# Orchestration (Command + Agent + Skill)

## What is it?

Orchestration is the pattern of combining commands, agents, and skills into a complete workflow. Instead of using each feature separately, you wire them together: a **command** kicks things off, an **agent** does the heavy work, and a **skill** enforces the standards.

This is the pattern: **Command -> Agent -> Skill**.

## Why use it?

Individual features are useful on their own. But real work usually involves multiple steps. Planning a week involves research, scheduling, and formatting. Building a feature involves planning, coding, testing, and reviewing. Orchestration lets you turn these multi-step processes into one-command workflows.

You type one slash command and Claude handles the entire pipeline.

## How it works

```
You type /command
    |
    v
Command defines the workflow
    |
    v
Command delegates to Agent(s)
    |
    v
Agent does the work, guided by Skill(s)
    |
    v
Result comes back to you
```

Each piece has a job:
- **Command** -- the entry point. Defines what to do and what agents to use.
- **Agent** -- the worker. Runs in its own context with specific tools.
- **Skill** -- the quality gate. Enforces standards the agent must follow.

## Example 1: For Everyone

You want a single command that **plans your week** -- looks at your tasks, organizes them by priority, and creates a daily schedule.

### The Skill: Weekly Planner Standards

Create `.claude/skills/weekly-planner/SKILL.md`:

```markdown
---
name: weekly-planner
description: Use when creating weekly plans, schedules, or task organization
---

# Weekly Planner Standards

When creating a weekly plan, follow these rules:

## Structure
- Group tasks by day (Monday through Friday)
- Maximum 3 big tasks per day
- Include buffer time between tasks
- Put the hardest task in the morning

## Format
- Use a simple table for each day
- Mark priority: high, medium, low
- Include estimated time for each task
- Add a "weekly goals" summary at the top

## Rules
- Never schedule more than 6 hours of focused work per day
- Always include a lunch break
- Leave Friday afternoon unscheduled for overflow
- If there are too many tasks, move low-priority ones to next week
```

### The Agent: Planner

Create `.claude/agents/weekly-planner.md`:

```markdown
---
name: weekly-planner
description: Plans and organizes weekly tasks into a structured schedule
model: sonnet
tools:
  - Read
  - Glob
  - Write
---

# Weekly Planner Agent

You plan and organize tasks into a weekly schedule.

1. Read any task files, todo lists, or notes in the current folder
2. Ask the user about any commitments or deadlines not in the files
3. Organize tasks by priority and estimated time
4. Create a weekly plan following the weekly-planner skill guidelines
5. Save the plan to `weekly-plan.md`
```

### The Command: Plan My Week

Create `.claude/commands/plan-my-week.md`:

```markdown
---
description: Create a weekly plan from your tasks and commitments
---

Help me plan my week.

1. Use the weekly-planner agent to analyze my tasks and create a schedule
2. Make sure the plan follows the weekly-planner skill standards
3. Save the result to weekly-plan.md
4. Show me the plan and ask if I want to adjust anything
```

### Use it

```bash
claude
/plan-my-week
```

One command. Claude reads your tasks, the agent organizes them, the skill enforces the format, and you get a clean weekly plan.

## Example 2: For Developers

You want a single command that runs a **full feature pipeline** -- plan the implementation, write the code, run tests, and review.

### The Skill: Code Standards

Create `.claude/skills/code-standards/SKILL.md`:

```markdown
---
name: code-standards
description: Use when writing or reviewing code in this project
---

# Code Standards

## Before writing code
- Check existing patterns in the codebase first
- Plan the approach before coding

## While writing code
- Add type hints to all function signatures
- Write docstrings for public functions only
- Follow existing naming conventions in the codebase
- Keep functions under 30 lines

## After writing code
- Run the test suite
- Check for linting errors
- Verify the feature works end-to-end
```

### The Agent: Feature Builder

Create `.claude/agents/feature-builder.md`:

```markdown
---
name: feature-builder
description: Builds features end-to-end with planning, implementation, testing, and review
model: opus
tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
---

# Feature Builder Agent

You build features from start to finish. For each feature:

1. **Plan** -- Read the codebase, understand existing patterns, outline the approach
2. **Implement** -- Write the code following the code-standards skill
3. **Test** -- Write tests and run them. Fix failures before proceeding.
4. **Review** -- Check your own work for bugs, missing edge cases, and style issues

Report back with:
- What you built
- What tests you added
- Any concerns or trade-offs
```

### The Command: Build Feature

Create `.claude/commands/build-feature.md`:

```markdown
---
description: Plan, implement, test, and review a new feature end-to-end
---

I want to build a new feature. Ask me what the feature is.

Then use the feature-builder agent to:
1. Plan the implementation
2. Write the code (following code-standards skill)
3. Write and run tests
4. Self-review the changes

Show me the plan before implementing. After implementation, show me a summary of what was built and the test results.
```

### Use it

```bash
claude
/build-feature
```

Claude asks what you want to build, plans the approach, implements it following your code standards, writes tests, runs them, reviews the code, and reports back. One command, full pipeline.

## Try it yourself

1. Pick either the "plan my week" or "build feature" example from [`examples/`](examples/)
2. Copy all three pieces (skill + agent + command) into your project's `.claude/` folder
3. Run the command and see the full pipeline in action

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- Start with the skill (standards), then the agent (worker), then the command (entry point). Build from the inside out.
- Keep each piece focused. One skill per concern. One agent per job. One command per workflow.
- The command is for the human (readable, simple). The agent is for Claude (detailed instructions). The skill is for quality (enforced standards).
- You can have a command use multiple agents for different steps.
- Test each piece separately before wiring them together.
- This pattern scales: add more agents and skills as your workflows grow.
