# Try it yourself: Agents (Subagents)

## Exercise

Create an agent that handles a task you regularly delegate or would like to automate.

### Step 1: Pick a task to delegate

Think of something you'd hand off to an assistant:
- "Research this topic for me"
- "Review this document for errors"
- "Summarize what happened today in this project"
- "Check if my code has any security issues"

### Step 2: Create the agents folder

```bash
cd /path/to/your/project
mkdir -p .claude/agents
```

### Step 3: Write your agent

Create a file named after the task. For example, `fact-checker.md`:

```bash
touch .claude/agents/fact-checker.md
```

Write the agent definition:

```markdown
---
name: fact-checker
description: Checks claims for accuracy using web search
model: sonnet
tools:
  - WebSearch
  - WebFetch
---

# Fact Checker

When given a claim or statement, you:
1. Search for reliable sources
2. Verify if the claim is accurate
3. Report: confirmed, false, or partially true
4. Cite your sources
```

### Step 4: Use it

Open Claude Code and say:

```
use the fact-checker agent to verify: "The Great Wall of China is visible from space"
```

### Step 5: Observe

Notice how the agent works in its own context. Your main conversation stays clean, and you just get the result back.
