# Large Repo Rules

Append these to universal.md when working in codebases with 1000+ files, monorepos, or deep directory trees.

## Navigation

- Never read a directory listing of the entire repo. Start from the area the user points you to.
- Use Glob with specific patterns (`src/auth/**/*.ts`) instead of broad ones (`**/*.ts`).
- When searching, scope to the relevant package/module — don't search the entire repo.
- Use the Explore agent for broad codebase questions instead of multiple manual searches.

## Context budget

- Don't read config files (tsconfig, eslint, package.json) unless the task requires them.
- Don't read test files to understand implementation. Read the source first.
- When a file is over 500 lines, read only the relevant section with offset/limit.
- Avoid loading entire dependency trees into context. Focus on the immediate layer.

## Changes

- Before creating a new file, search for existing files that serve a similar purpose.
- Follow the existing project structure. Don't invent new directories without asking.
- When modifying a shared utility, check for existing callers before changing its signature.
- Keep PRs focused. One logical change per commit, not a kitchen-sink refactor.

## Monorepo

For monorepo-specific rules, see `templates/monorepo.md`.
