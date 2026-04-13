# Try it yourself: Settings & Permissions

## Exercise

Set up a settings file that matches your workflow.

### Step 1: Create the settings file

```bash
cd /path/to/your/project
mkdir -p .claude
touch .claude/settings.local.json
```

Using `settings.local.json` so your experiments don't affect teammates.

### Step 2: Add your most-used commands to allow

Think about which commands you approve most often. Add them:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep"
    ]
  }
}
```

### Step 3: Test it

Start Claude Code and ask it to read a file. It should proceed without asking permission.

```bash
claude
```

### Step 4: Add deny rules

Think about what Claude should never do in your project. Add deny rules:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep"
    ],
    "deny": [
      "Bash(rm *)",
      "Edit(.env)"
    ]
  }
}
```

### Step 5: Test the deny rules

Ask Claude to delete a file or edit .env. It should refuse.

### Step 6: Check your settings

Inside Claude Code, type `/config` to see all active settings or `/permissions` to see permission rules.
