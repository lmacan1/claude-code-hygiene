# Hooks

Hooks are shell commands that Claude Code runs automatically in response to specific events. They enforce project rules without relying on Claude to remember them.

Configuration lives in `.claude/settings.json` under the `hooks` key, or in `~/.claude/settings.json` for global hooks.

## Configuration format

```json
{
  "hooks": {
    "<event>": [
      {
        "command": "shell command to run",
        "timeout": 10000
      }
    ]
  }
}
```

- `command`: any shell command. Receives event data via stdin as JSON.
- `timeout`: milliseconds before the hook is killed. Default: 60000.

## Key events

| Event | Fires when | Common use |
|---|---|---|
| `PreToolUse` | Before a tool runs | Block dangerous operations, enforce file restrictions |
| `PostToolUse` | After a tool completes | Auto-format, lint changed files |
| `Notification` | Claude sends a notification | Pipe to Slack, desktop alert |
| `Stop` | Claude finishes a response | Run validation, update status |
| `SubagentStop` | A subagent completes | Aggregate results, trigger next step |
| `UserPromptSubmit` | User submits a prompt | Preprocess input, add context |

For the full event list, see the [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code/hooks).

## Practical examples

### Auto-format on file write

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "command": "if [ \"$TOOL_NAME\" = 'Write' ] || [ \"$TOOL_NAME\" = 'Edit' ]; then npx prettier --write \"$FILE_PATH\"; fi",
        "timeout": 10000
      }
    ]
  }
}
```

### Block writes to protected files

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "command": "echo $TOOL_INPUT | jq -r '.file_path // empty' | grep -qE '(package-lock\\.json|\\.env)' && echo 'BLOCK: Do not modify this file' && exit 1 || exit 0",
        "timeout": 5000
      }
    ]
  }
}
```

### Lint after commit

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "command": "if echo $TOOL_INPUT | jq -r '.command // empty' | grep -q 'git commit'; then npm run lint; fi",
        "timeout": 30000
      }
    ]
  }
}
```

### Notification to terminal

```json
{
  "hooks": {
    "Notification": [
      {
        "command": "osascript -e 'display notification \"Claude Code finished\" with title \"Claude\"'",
        "timeout": 5000
      }
    ]
  }
}
```

See `examples/hooks/` for ready-to-use hook configs.

## Anti-patterns

- **Hooks on every tool call with no filtering.** A slow hook on `PreToolUse` that runs for every tool — not just writes — adds latency to everything. Always filter by tool name or input.
- **Hooks that swallow errors.** If a hook fails silently (exits 0 on error), it provides false confidence. Let hooks fail loudly.
- **Hooks that modify files Claude is about to read.** Race conditions. If you need to transform files, do it in `PostToolUse` after the write, not `PreToolUse` before the read.
- **Complex logic in inline shell.** If your hook command is over 2 lines, put it in a script file and call that instead.
- **Hooks with no timeout.** A hung hook blocks Claude indefinitely. Always set a reasonable timeout.
