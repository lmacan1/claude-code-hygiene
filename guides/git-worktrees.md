# Git Worktrees

Worktrees let you have multiple branches checked out simultaneously in separate directories. Each directory gets its own Claude Code session with isolated context.

## What worktrees enable

- Work on a feature branch while keeping main clean in another directory
- Run parallel Claude sessions on different tasks without context interference
- Test changes in isolation without stashing or committing WIP
- Subagents can use worktrees via `isolation: "worktree"` to make changes without affecting your working directory

## Setup

### Claude Code's built-in worktree support

Launch a subagent with `isolation: "worktree"` and it gets its own copy of the repo:

```
Agent(isolation="worktree", prompt="Refactor the auth module to use JWT")
```

The agent works in a temporary worktree. If it makes changes, the worktree path and branch are returned. If it makes no changes, the worktree is auto-cleaned.

### Manual worktree creation

```bash
# Create a worktree for a feature branch
git worktree add ../my-feature feature-branch

# Create a worktree with a new branch
git worktree add ../my-feature -b feature-branch

# List active worktrees
git worktree list

# Remove a worktree when done
git worktree remove ../my-feature
```

## Workflow

1. **Main directory** — stays on `main` or your primary branch. Use for reviews, quick fixes, reference.
2. **Feature worktrees** — one per feature branch. Run a Claude session in each.
3. **Experiment worktrees** — throwaway explorations. Delete when done.

```
~/project/              ← main branch, stable
~/project-feature-auth/ ← auth feature, active development
~/project-experiment/   ← throwaway exploration
```

Each directory is a full working copy. Run `claude` in any of them independently.

## Cleanup

```bash
# Remove a specific worktree
git worktree remove ../my-feature

# Remove all stale worktrees (where the directory was deleted)
git worktree prune

# Force-remove a worktree with uncommitted changes
git worktree remove --force ../my-feature
```

Add worktree directories to your project's `.gitignore` or global gitignore if they share a parent directory.

## When to use worktrees vs branches

- **Worktrees** — when you need two branches checked out at the same time (parallel Claude sessions, comparing implementations, isolated subagent work)
- **Regular branches** — when you're working on one thing at a time and switching between tasks sequentially
