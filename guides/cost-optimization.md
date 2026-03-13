# Cost Optimization

All cost levers for Claude Code, ranked by impact. This is the numbers-focused companion to `guides/context-window.md`.

## Strategy ranking

| Strategy | Savings estimate | Effort | Notes |
|---|---|---|---|
| Plan mode for exploration | ~50% | None | Just press Shift+Tab before exploring |
| Hybrid models (Opus + Haiku subagents) | 40-70% | Low | Use `model` param on Agent tool |
| `.claudeignore` | 15-30% | 5 min setup | One-time per project |
| Structured prompts | 10-20% | Habit change | Include file paths, constraints upfront |
| `/compact` timing | Extends session 30-50% | Habit change | Compact after exploration, not mid-task |
| Subagent isolation | Variable | Moderate | Biggest win in large repos |
| **Combined stack** | **55-80%** | **Moderate** | All of the above together |

## Plan mode

The single biggest lever for exploration-heavy tasks.

- Exploration happens in a planning context that doesn't commit to implementation
- You review the plan, then Claude implements with only the relevant files loaded
- Works best for: unfamiliar code, multi-file changes, architecture decisions
- Skip for: obvious single-file edits, tasks where you already know the exact change

## Hybrid models

Use expensive models for thinking, cheap models for doing:

- **Opus** for main conversation: complex decisions, architecture, nuanced implementation
- **Sonnet** for subagents: code search, test running, formatting, simple changes
- **Haiku** for Explore agents: file search, pattern matching, codebase questions

The model parameter on Agent tool calls controls this. No project configuration needed.

## .claudeignore

Prevents Claude from reading files that waste context. Create once, benefit every session.

Run the `claudeignore-generator` skill to auto-detect your stack and generate one.

Highest-value patterns to exclude:
- Build output (`dist/`, `build/`, `.next/`)
- Dependencies (`node_modules/`, `.venv/`)
- Generated files (`*.lock`, `*.min.js`, `*.map`)
- Large data files (DB dumps, fixtures, seed data)

## Session management

- One task per session. Context accumulates — a fresh session for a new task is cheaper than compacting a stale one.
- `/compact` after exploration phases, before implementation.
- Start a new session at ~80% context usage. Quality degrades past this point, and you'll waste tokens on degraded output.
- Don't restart sessions mid-implementation. The cost of re-establishing context outweighs the savings.

## What doesn't work

Common advice that sounds good but doesn't actually save tokens:

- **"Keep prompts short."** Prompt length is a tiny fraction of total cost. A detailed prompt that prevents two rounds of clarification is cheaper than a terse one.
- **"Avoid reading files."** Claude needs context to make good changes. Reading the right files upfront is cheaper than guessing wrong and fixing mistakes.
- **"Use only Haiku."** Cheap models make more mistakes, requiring more rounds of correction. Use Haiku for search, not for implementation.
- **"Disable tools to save tokens."** Tools are how Claude gets work done. Disabling them forces verbose workarounds that cost more.
- **"Start a new session constantly."** Re-establishing context has a cost. Only start fresh when the current session is genuinely degraded or the task is complete.
