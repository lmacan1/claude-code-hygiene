# Subagents

Subagents are separate Claude instances that run in their own context window. They can explore, research, and execute tasks without loading results into your main conversation.

## Built-in agent types

| Type | Model | Tools | Use for |
|---|---|---|---|
| `Explore` | Haiku (fast) | Read-only (Glob, Grep, Read) | Codebase search, file discovery, pattern analysis |
| `Plan` | Same as parent | Read-only | Architecture decisions, implementation planning |
| `general-purpose` | Same as parent | All tools | Multi-step tasks that need file writes, bash, etc. |

### Explore agents

Cheapest and fastest. Use for any read-only research:
- "Find all files that import UserService"
- "What testing patterns does this project use?"
- "List all API endpoints and their auth requirements"

### Plan agents

Same read access as Explore, but designed for architectural thinking:
- "Design the data model for a notification system"
- "Plan how to refactor the auth module into separate services"

### General-purpose agents

Full tool access. Use when the task requires writing files or running commands:
- "Add error handling to all API routes in src/api/"
- "Run the test suite and fix any failures"

## When to use subagents

**Use subagents when:**
- You need to explore many files but only need a summary
- Multiple independent research tasks can run in parallel
- The task would pollute main context with files you won't need again
- You want to isolate risky operations (experiments, speculative changes)

**Don't use subagents when:**
- The task is a single file read or simple search (use Grep/Glob directly)
- You need the subagent's full output in main context anyway
- The task requires back-and-forth with the user
- Tasks share state or depend on each other's results

## Parallelization

Fan out multiple subagents when tasks are independent:

```
Good — independent files, no shared state:
  Agent 1: "Analyze auth module patterns"
  Agent 2: "Analyze payment module patterns"
  Agent 3: "Analyze notification module patterns"

Bad — tasks depend on each other:
  Agent 1: "Design the API schema"
  Agent 2: "Implement the API based on Agent 1's schema"  ← needs Agent 1's output
```

Launch parallel agents in a single message with multiple tool calls. Don't launch them sequentially.

## Model selection for cost savings

Mix models based on task complexity:

- **Main conversation:** Opus for complex implementation, architecture decisions
- **Subagents:** Sonnet or Haiku for exploration, search, simple tasks

You can specify the model when launching a subagent with the `model` parameter:

```
Agent(model="haiku", prompt="Find all usages of the deprecated auth middleware")
```

**Typical savings:** 40-70% cost reduction on exploration-heavy workflows by using Haiku subagents for research and Opus for implementation.

## Custom agents

Define reusable agents in `.claude/agents/` for repeated workflows:

```markdown
---
name: test-runner
description: Run tests for the affected module and report results
model: sonnet
---

Run the test suite for the module that was most recently modified.
Report: number of tests run, passed, failed, and any error messages.
```

Custom agents are useful when:
- You repeat the same type of task frequently
- The task has specific instructions that would be verbose to type each time
- You want consistent behavior across team members

## Anti-patterns

- **Over-parallelization.** Launching 10 subagents for tasks that could be 3 Grep calls. Subagents have overhead — use them when the task justifies it.
- **Subagents for trivial tasks.** "Use a subagent to read this one file" — just read the file directly.
- **Ignoring subagent results.** Launch a research agent, then redo the same research manually. Trust the subagent's output.
- **Shared state assumptions.** Subagents don't share context with each other. If Agent 2 needs Agent 1's output, run them sequentially.
