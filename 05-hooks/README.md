# Hooks

## What is it?

Hooks are scripts that run automatically when Claude does specific things. Before Claude edits a file, after it runs a command, when a session starts -- you can attach a script to any of these moments.

Think of hooks like tripwires. You set them up once, and they fire every time the right event happens. Claude doesn't control them -- your system does.

## Why use it?

Claude is powerful but it doesn't know everything about your workflow. Maybe you want to auto-format code after every edit. Maybe you want a notification when Claude finishes a long task. Maybe you want to block Claude from touching certain files. Hooks let you add these automations without asking Claude to remember them.

The key difference from CLAUDE.md rules: hooks are **enforced by the system**, not by Claude. Claude can't ignore a hook the way it might occasionally skip a CLAUDE.md instruction.

## How to set it up

Hooks are configured in your `settings.json` file -- not as separate files. Add a `"hooks"` section:

**Project-wide** (shared with team): `.claude/settings.json`
**Personal** (just you): `.claude/settings.local.json`
**Global** (all projects): `~/.claude/settings.json`

The basic structure:

```json
{
  "hooks": {
    "EventName": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "your-script-here"
          }
        ]
      }
    ]
  }
}
```

### Common hook events

| Event | When it fires |
|-------|--------------|
| `PreToolUse` | Before Claude uses a tool (can block it) |
| `PostToolUse` | After Claude uses a tool |
| `Notification` | When Claude sends a notification |
| `Stop` | When Claude finishes its turn |
| `SessionStart` | When a session begins |
| `UserPromptSubmit` | When you send a message |

### Exit codes

Your script's exit code tells Claude what to do:

- **Exit 0** -- success, carry on. Anything printed to stdout gets added to Claude's context.
- **Exit 2** -- block the action. Print a reason to stderr and Claude receives it as feedback.
- **Any other code** -- action proceeds but shows a warning.

## Example 1: For Everyone

You want a **desktop notification** when Claude finishes a long task so you don't have to keep watching the screen.

Add this to your `.claude/settings.local.json`:

**On Linux:**

```json
{
  "hooks": {
    "Notification": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' \"$(echo $CLAUDE_NOTIFICATION | jq -r '.message')\" 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

**On macOS:**

```json
{
  "hooks": {
    "Notification": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e \"display notification \\\"Claude finished a task\\\" with title \\\"Claude Code\\\"\""
          }
        ]
      }
    ]
  }
}
```

Now whenever Claude sends a notification (like finishing a task), your system pops up a notification. You can walk away and come back when it's done.

**Without hooks:** You stare at the terminal waiting for Claude to finish.

**With hooks:** You get a system notification and can work on something else.

## Example 2: For Developers

You want to **auto-format code** every time Claude edits or creates a file, so you never have to worry about formatting.

Add this to your `.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

This hook fires after every `Edit` or `Write` tool use. It reads the file path from the tool input and runs Prettier on it. The `|| true` ensures it doesn't break if Prettier isn't installed or the file isn't supported.

You can also **block Claude from editing protected files**. Add a `PreToolUse` hook:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path') && case \"$FILE\" in *.env|*credentials*|*secret*) echo \"Blocked: $FILE is a protected file\" >&2; exit 2;; esac"
          }
        ]
      }
    ]
  }
}
```

Exit code 2 blocks the action. Claude sees "Blocked: .env is a protected file" and stops trying to edit it.

## Try it yourself

1. Pick one of the examples from [`examples/`](examples/)
2. Copy the JSON into your `.claude/settings.local.json`
3. Start a Claude Code session and trigger the hook
4. Watch it fire automatically

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- Use `matcher` to filter which tools trigger a hook. `"Edit|Write"` matches both. Without a matcher, the hook fires for every tool use of that event type.
- Hooks run outside Claude's context. They are system-enforced -- Claude can't skip them.
- Use `PreToolUse` with exit code 2 to block dangerous actions. Use `PostToolUse` for cleanup.
- Print to stdout (exit 0) to inject context into Claude's conversation. Great for reminders after compaction.
- Keep hook scripts fast. They run synchronously and a slow hook slows down Claude.
- Test hooks with simple `echo` commands first before adding complex logic.
