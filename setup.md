# Hygiene Setup

You are setting up Claude Code hygiene for the current project using the claude-code-hygiene knowledge base.

## Steps

1. Clone the knowledge base if not already present:
   - Run: `git clone https://github.com/lukamacan/claude-code-hygiene /tmp/claude-code-hygiene 2>/dev/null || git -C /tmp/claude-code-hygiene pull`
   - All template/example paths below are relative to `/tmp/claude-code-hygiene/`

2. Read the current project root to detect the stack:
   - `next.config.*` / `package.json` with react → frontend
   - `pyproject.toml` / `setup.py` / `requirements.txt` → python
   - `go.mod` → go
   - `Cargo.toml` → rust
   - Check if monorepo: `pnpm-workspace.yaml`, `nx.json`, `package.json` workspaces field
   - Check total file count: over 1000 files → large repo

3. Build CLAUDE.md by concatenating the right templates:
   - Always start with `templates/universal.md`
   - Add `templates/large-repo.md` if 1000+ files
   - Add `templates/frontend.md` if frontend project
   - Add `templates/api.md` if backend/API project
   - Add `templates/monorepo.md` if monorepo
   - If the project already has a CLAUDE.md, preserve any project-specific rules and append the template rules below them under a `## Hygiene rules` header

4. Generate `.claudeignore` based on the detected stack:
   - Use `examples/claudeignore/nextjs` as a starting point for Next.js/React projects
   - Use `examples/claudeignore/python` as a starting point for Python projects
   - For other stacks, follow the pattern reference in `skills/claudeignore-generator.md`
   - If `.claudeignore` already exists, merge new patterns in without duplicating

5. Show the user what you're about to write before writing:
   - The assembled CLAUDE.md content
   - The generated .claudeignore content
   - Which templates were selected and why

6. Write the files after user approval.
