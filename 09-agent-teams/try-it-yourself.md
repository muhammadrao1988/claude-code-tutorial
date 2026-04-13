# Try it yourself: Agent Teams

## Exercise

Set up agent teams and run a parallel review.

### Step 1: Enable agent teams

Add this to your `.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### Step 2: Pick a task

Choose something that benefits from multiple perspectives:
- Review a recent PR or set of changes
- Research a technical decision (which database, which framework)
- Investigate a bug from different angles
- Plan a feature by exploring different approaches

### Step 3: Create the team

Start Claude Code and describe your team:

```bash
claude
```

Then type:

```
Create an agent team with 3 teammates to review the recent changes
in this project. One should focus on correctness, one on readability,
and one on potential edge cases.
```

### Step 4: Interact with teammates

Press `Shift+Down` to cycle through teammates. You can message any teammate directly to give extra instructions or ask follow-up questions.

### Step 5: Observe the coordination

Watch how teammates:
- Claim tasks from the shared task list
- Share findings with each other
- Challenge each other's conclusions

### Step 6: Clean up

When done, tell the lead:

```
Clean up the team
```

### Bonus: Try split panes

If you have tmux installed, try split-pane mode:

```bash
claude --teammate-mode tmux
```

Each teammate gets its own terminal pane so you can see everyone's output at once.
