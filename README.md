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
CLAUDE.md                      # The rules this repo itself uses (dogfooding)
templates/
  universal.md                 # Base token efficiency rules — works in any repo
  large-repo.md                # Extra rules for big codebases (monorepos, 1000+ files)
skills/
  claude-md-trimmer.md         # Skill: analyzes bloated CLAUDE.md, splits + rewrites lean
```

## How to use it

**Quick start:** Copy `templates/universal.md` into your project's `CLAUDE.md`. Done.

**Large codebases:** Start with `universal.md`, then append the rules from `large-repo.md`.

**Already have a CLAUDE.md?** Run the `claude-md-trimmer` skill to audit it and split bloated sections into separate reference files.

## Contributing

Open an issue or PR. Keep rules practical and tested — no theoretical "best practices" that haven't been validated in real usage.

## License

MIT
