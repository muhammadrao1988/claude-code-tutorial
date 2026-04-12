# Try it yourself: Slash Commands

## Exercise

Create your first custom command for something you do repeatedly.

### Step 1: Think of a repeated task

What do you ask Claude to do more than once a week? Examples:
- "Summarize what changed today"
- "Check this file for errors"
- "Write a commit message for my changes"
- "Explain this code to me simply"

### Step 2: Create the commands folder

```bash
cd /path/to/your/project
mkdir -p .claude/commands
```

### Step 3: Write your command

Create a file for your task. For example, `daily-summary.md`:

```bash
touch .claude/commands/daily-summary.md
```

Write the prompt you normally type out, but save it in the file:

```markdown
---
description: Show what changed in this project today
---

Look at the git log for today's commits.
For each commit, write a one-line summary.
Group them by author if there are multiple.
End with a count of total files changed.
```

### Step 4: Use it

Open Claude Code in your project:

```bash
claude
```

Type `/daily-summary` and watch it run your saved prompt.

### Step 5: Improve it

After using it a few times, adjust the prompt. Add more detail, remove things you don't need. Your commands get better with use.
