# Backlog

This folder tracks tutorial topics — what's planned and what's done.

## Structure

- `pending/` — topics waiting to be written
- `completed/` — topics that have been published

## Format

Each file uses frontmatter with topic, priority, source, date_added, and status.

## Workflow

1. New topics get added to `pending/` (manually or by the tutorial-researcher agent)
2. When a tutorial is being written, the backlog item stays in `pending/` with status "in-progress"
3. When published, the `/publish` command moves it to `completed/`

## Adding Topics

Use the tutorial-researcher agent to scan for new Claude Code features, or add manually:

```bash
# In Claude Code
# Ask the tutorial-researcher agent to scan for new features
```

Or create a file in `pending/` following the format in any existing file.
