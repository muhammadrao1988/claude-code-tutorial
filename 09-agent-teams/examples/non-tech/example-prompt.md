# Agent Teams — Non-Tech Example Prompt

## Step 1: Enable agent teams

Copy `settings.json` into your `.claude/settings.json` to enable agent teams.

## Step 2: Use this prompt in Claude Code

```
Create an agent team to help plan a company team offsite for 20 people.
Spawn 3 teammates:
- One focused on venue and logistics (location, dates, travel)
- One focused on activities and agenda (team building, workshops, meals)
- One playing devil's advocate (budget concerns, what could go wrong, backup plans)

Have them research and share findings with each other.
When all three are done, synthesize their findings into a single plan document.
```

## What to expect

- Claude creates a team lead and 3 teammates
- Each teammate researches their area independently
- They share findings and challenge each other
- The lead produces a combined plan at the end
- Press Shift+Down to cycle through teammates and see their progress
