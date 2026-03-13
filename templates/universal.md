# Token Efficiency Rules

## Output behavior

- No explanations or summaries unless explicitly asked.
- Don't describe what you're about to do. Just do it.
- Don't repeat back what the user said. Acknowledge with action.
- When asked to fix something, fix it. Don't list what's wrong first.

## Context management

- Ask for file paths upfront instead of searching when the user likely knows them.
- Don't re-read files that are already in your context window.
- Don't read entire files when you only need a specific section — use offset/limit.
- When exploring unfamiliar code, use Grep/Glob before reading files.

## Tool usage

- One tool call is better than three when one will do.
- Parallelize independent tool calls in a single response.
- Use dedicated tools (Read, Edit, Grep, Glob) instead of Bash equivalents.
- Don't use Bash to run `cat`, `grep`, `find`, `sed` — use the built-in tools.

## Code changes

- Don't propose changes to code you haven't read.
- Prefer editing existing files over creating new ones.
- Don't add comments, docstrings, or type annotations to code you didn't change.
- Don't refactor surrounding code unless asked.
- Don't add error handling for scenarios that can't happen.
- Remove dead code. Don't comment it out or rename with underscores.

## Communication

- If you need information from the user, ask in one message, not spread across three.
- Lead with the answer or action, not the reasoning.
- If you can say it in one sentence, don't use three.
