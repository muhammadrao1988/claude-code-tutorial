# Agent Teams — Developer Example Prompt

## Step 1: Enable agent teams

Copy `settings.json` into your `.claude/settings.json` to enable agent teams.

## Step 2: Parallel code review — use this prompt

```
Create an agent team to review the changes in this branch.
Spawn 3 teammates:
- One focused on security (auth, input validation, exposed secrets)
- One focused on performance (queries, loops, memory usage)
- One focused on test coverage (missing tests, edge cases, mocking)

Have them each review and report findings. They should challenge
each other's findings if they disagree.
```

## Step 3: Debugging with competing hypotheses — use this prompt

```
Users report the app crashes after login. Spawn 4 teammates to
investigate different hypotheses:
- Token expiration issue
- Database connection timeout
- Memory leak in session handling
- Race condition in auth middleware

Have them talk to each other to disprove each other's theories.
Update the findings doc with whatever consensus emerges.
```

## What to expect

- Each teammate works in its own context with full tool access
- They communicate directly with each other (not just through the lead)
- The shared task list shows progress across all teammates
- Use Shift+Down to cycle through teammates, or use tmux for split panes:
  ```bash
  claude --teammate-mode tmux
  ```
