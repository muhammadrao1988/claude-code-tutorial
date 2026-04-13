# Try it yourself: Hooks

## Exercise

Create a hook that logs every tool Claude uses to a file.

### Step 1: Create the settings file

```bash
cd /path/to/your/project
mkdir -p .claude
touch .claude/settings.local.json
```

### Step 2: Add a logging hook

Open `.claude/settings.local.json` and paste:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date '+%H:%M:%S') - Tool used: $(jq -r '.tool_name')\" >> .claude-tool-log.txt"
          }
        ]
      }
    ]
  }
}
```

### Step 3: Test it

Start Claude Code and ask it to do something (read a file, search for text, etc.):

```bash
claude
```

### Step 4: Check the log

After Claude uses a few tools, check your log file:

```bash
cat .claude-tool-log.txt
```

You should see entries like:
```
14:23:01 - Tool used: Read
14:23:03 - Tool used: Grep
14:23:05 - Tool used: Edit
```

### Step 5: Experiment

Try changing `PostToolUse` to `PreToolUse` and adding `exit 2` to block specific tools. See how Claude reacts when a tool is blocked.
