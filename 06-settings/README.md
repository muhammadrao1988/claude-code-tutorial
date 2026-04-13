# Settings & Permissions

## What is it?

Settings control how Claude Code behaves -- what it's allowed to do, which model it uses, how permissions work, and what gets sandboxed. Everything is configured through `settings.json` files.

Think of settings as the control panel for Claude Code. You decide how much freedom Claude gets and how it operates in your project.

## Why use it?

Out of the box, Claude Code asks for permission on almost every action. That's safe but slow. With settings, you can pre-approve safe actions (like reading files), block dangerous ones (like deleting things), and skip the constant permission prompts for commands you trust.

Settings also let teams enforce consistent behavior. Everyone on the team gets the same rules when they clone the project.

## How to set it up

Settings live in JSON files at three levels:

| File | Scope | Shared? |
|------|-------|---------|
| `~/.claude/settings.json` | All your projects | No (personal) |
| `.claude/settings.json` | This project (everyone) | Yes (commit to git) |
| `.claude/settings.local.json` | This project (just you) | No (gitignored) |

Project settings override global settings. Create whichever level you need:

```bash
mkdir -p .claude
touch .claude/settings.json
```

## Example 1: For Everyone

You're tired of Claude asking permission for basic things like reading files and running simple commands. You want to **reduce permission prompts** while keeping dangerous actions blocked.

Create `.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(ls *)",
      "Bash(cat *)",
      "Bash(pwd)",
      "Bash(date)"
    ],
    "deny": [
      "Bash(rm *)",
      "Bash(sudo *)",
      "Bash(chmod *)"
    ]
  }
}
```

**What this does:**
- `allow` -- these tools run without asking. Claude can read files, search, and run safe commands instantly.
- `deny` -- these are always blocked. Claude can't delete files, use sudo, or change permissions. Even if you ask it to.

**Without settings:** Claude asks "Can I read this file?" every time. You click approve 50 times a day.

**With settings:** Reading is instant. Dangerous commands are blocked. You only get prompted for things in the middle.

### Permission modes

You can also set a global permission mode instead of individual rules:

| Mode | What it does |
|------|-------------|
| `default` | Asks permission for everything (the default) |
| `acceptEdits` | Auto-approves file edits and safe shell commands |
| `plan` | Read-only mode -- Claude can analyze but not change anything |
| `auto` | Smart classifier decides what's safe (experimental) |

Set it in settings or use the CLI flag:

```bash
claude --permission-mode acceptEdits
```

## Example 2: For Developers

You want a **full project setup** with model selection, sandboxing, permissions, and environment variables.

Create `.claude/settings.json` for the team:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm run *)",
      "Bash(npx *)",
      "Bash(git status)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Edit(/src/**)",
      "Edit(/tests/**)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Edit(.env)",
      "Edit(**/credentials*)"
    ]
  },
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

And `.claude/settings.local.json` for your personal preferences (gitignored):

```json
{
  "permissions": {
    "allow": [
      "Bash(docker compose *)",
      "Bash(make *)"
    ]
  }
}
```

**What this does:**

- **allow** -- npm/npx commands, git read commands, and file edits in `src/` and `tests/` run without prompts
- **deny** -- force pushes, .env edits, and `rm -rf` are always blocked
- **env** -- enables experimental features like Agent Teams
- **local settings** -- your personal Docker and Make commands are approved, but teammates who don't use Docker won't see them

### Permission rule syntax

Rules use glob patterns:

```
Bash(npm run *)         -- any npm run command
Bash(git *)             -- any git command
Edit(/src/**)           -- any file under src/, recursively
Edit(*.env)             -- any .env file
WebFetch(domain:api.example.com)  -- specific domain only
```

The order of precedence is: **deny > ask > allow**. Deny always wins.

## Try it yourself

1. Copy one of the examples from [`examples/`](examples/) into your project's `.claude/` folder
2. Start Claude Code and notice fewer permission prompts
3. Try to do something that's denied -- Claude should refuse

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- Start permissive with `allow`, then add `deny` rules as you find things that should be blocked.
- Use `.claude/settings.json` for team rules (committed to git). Use `.claude/settings.local.json` for personal preferences (gitignored).
- The `deny` list always wins. If something is in both `allow` and `deny`, it gets denied.
- Use `/permissions` in Claude Code to see and modify permission rules during a session.
- Use `/config` to see current settings and change them interactively.
- Experiment with `acceptEdits` mode if you trust Claude with file changes -- it dramatically reduces prompts.
