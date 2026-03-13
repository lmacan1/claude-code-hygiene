# Context Window Management

The single highest-impact practice for Claude Code efficiency. Every strategy here reduces token waste and extends how long a session stays useful.

## .claudeignore

A `.claudeignore` file at your project root tells Claude which files to skip. Works like `.gitignore`. Without it, Claude reads build artifacts, lock files, and generated code that burn context for zero value.

**Starter patterns (copy and adapt):**

```
node_modules/
dist/
build/
.next/
coverage/
*.lock
*.min.js
*.map
*.generated.*
```

Framework-specific examples: `examples/claudeignore/`

**Impact:** 15-30% token savings depending on project size.

## Plan mode

Use plan mode (`/plan` or Shift+Tab) for tasks that need exploration before implementation.

- Claude explores the codebase, reads files, and drafts a plan — all in a cheaper planning context
- Implementation happens after the plan is approved, with only the relevant context loaded
- Saves ~50% on exploration-heavy tasks

**When to use:**
- Unfamiliar codebase areas
- Multi-file changes where you're not sure which files are involved
- Architecture decisions before writing code

**When to skip:**
- You already know exactly what to change
- Single-file edits
- Tasks where the plan is obvious

## Subagents for context isolation

Subagents run in separate context windows. Use them to explore without polluting your main session.

**Key pattern:** Explore in subagent → implement in main context.

```
"Search the codebase for all usages of UserService and summarize the patterns"
→ Subagent reads 15 files, returns a 20-line summary
→ Main context only sees the summary, not the 15 files
```

- Use `Explore` agent type for read-only codebase research
- Use `general-purpose` agent for tasks that need full tool access
- Fan out multiple subagents for independent research tasks

**Impact:** Keeps main context clean. Especially valuable in large repos.

For full subagent patterns, see `guides/subagents.md`.

## When to /compact

`/compact` summarizes your conversation to free context space. Use it:

- **After exploration** — once you've found what you need, compact before implementing
- **After large file reads** — if you loaded multiple large files to understand something
- **When quality degrades** — if Claude starts repeating itself, forgetting earlier context, or giving vague answers
- **Proactively at ~60% usage** — don't wait until the window is full

**Don't use it:**
- Mid-implementation when Claude needs to remember specific code details
- Right after giving complex instructions that haven't been acted on yet

## Monitoring token usage

- Check the status bar for current context usage percentage
- Start a new session when you hit ~80% — compacting past this point loses too much context
- One focused session per task beats one marathon session for three tasks

## Structured prompts

Front-load your prompts with the key information Claude needs:

```
Fix the auth timeout bug in src/auth/session.ts.
The issue: sessions expire after 30 min even when active.
Expected: sessions should extend on activity.
Don't modify the session store interface.
```

Not:

```
So I've been having this issue where users keep getting logged out and I think
it might be related to the session handling, maybe in the auth folder somewhere?
Can you take a look and see what's going on?
```

Specific prompts → fewer exploration cycles → less token waste.

## The combined stack

No single strategy is enough. The real savings come from combining them:

| Strategy | Solo savings | Combined with others |
|---|---|---|
| `.claudeignore` | 15-30% | Foundation layer |
| Plan mode | ~50% on exploration | Reduces what enters context |
| Subagents | Variable | Isolates exploration cost |
| Structured prompts | 10-20% | Fewer wasted cycles |
| `/compact` timing | Extends session life | Preserves quality longer |

**Realistic combined savings: 55-80%** depending on project size and task complexity.

## Checklist

Quick reference — do these for every project:

- [ ] `.claudeignore` exists and covers build artifacts, generated files, lock files
- [ ] Use plan mode for exploration-heavy tasks
- [ ] Explore in subagents, implement in main context
- [ ] `/compact` after exploration, before implementation
- [ ] Start new session before hitting 80% context
- [ ] Prompts include file paths, constraints, and expected behavior
