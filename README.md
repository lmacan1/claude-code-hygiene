# claude-code-hygiene

Practical rules, templates, and skills for keeping Claude Code efficient and on-task.

## What this is

A public knowledge base of battle-tested patterns for working with Claude Code. The focus is **token efficiency** — making Claude do more useful work per dollar spent.

Most CLAUDE.md files are bloated with rules Claude already follows by default, or with explanations that burn tokens every time they're loaded into context. This repo contains the lean versions.

## Who it's for

Anyone using Claude Code who:

- Pays for their own tokens and wants to stop wasting them
- Works on large codebases where context window management matters
- Wants a starting point for CLAUDE.md instead of writing one from scratch

## Repo structure

```
templates/
  universal.md                 # Base token efficiency rules — works in any repo
  large-repo.md                # Extra rules for big codebases (1000+ files)
  frontend.md                  # Rules for React, Next.js, Vue, Svelte projects
  api.md                       # Rules for REST/GraphQL API and backend projects
  monorepo.md                  # Rules for monorepo workspaces

guides/
  context-window.md            # Flagship guide — all context management strategies
  hooks.md                     # Claude Code hooks: config, events, examples
  subagents.md                 # When and how to use subagents effectively
  cost-optimization.md         # All cost levers ranked by impact
  git-worktrees.md             # Parallel branches with isolated Claude sessions

skills/
  claude-md-trimmer.md         # Skill: audit + rewrite bloated CLAUDE.md files
  claude-md-audit.md           # Skill: read-only audit with waste ratio report
  claudeignore-generator.md    # Skill: auto-detect stack, generate .claudeignore

examples/
  claudeignore/
    nextjs                     # .claudeignore for Next.js projects
    python                     # .claudeignore for Python projects
  hooks/
    pre-commit-lint.json       # Hook config: lint on commit and format on save
```

## How to use it

**Quick start:** Copy `templates/universal.md` into your project's `CLAUDE.md`. Done.

**Large codebases:** Start with `universal.md`, then append the rules from `large-repo.md`.

**Frontend / API projects:** Also append `templates/frontend.md` or `templates/api.md`.

**Monorepos:** Stack `universal.md` + `large-repo.md` + `monorepo.md`.

**Already have a CLAUDE.md?** Run the `claude-md-audit` skill for a read-only report, or the `claude-md-trimmer` skill to split and rewrite it.

**Need a .claudeignore?** Run the `claudeignore-generator` skill to auto-detect your stack and generate one, or copy from `examples/claudeignore/`.

**Want to understand the full picture?** Read `guides/context-window.md` for all context management strategies, or `guides/cost-optimization.md` for the numbers-focused summary.

## Contributing

Open an issue or PR. Keep rules practical and tested — no theoretical "best practices" that haven't been validated in real usage.

## License

MIT
