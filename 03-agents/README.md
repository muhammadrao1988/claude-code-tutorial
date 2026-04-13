# Agents (Subagents)

## What is it?

Agents are separate AI workers you can delegate tasks to. Each agent runs in its own isolated context with its own tools and instructions. You define an agent in a markdown file, and Claude can spin it up when the task fits.

Think of it like hiring a specialist. Instead of doing everything yourself, you hand off a specific job to someone who is set up for exactly that.

## Why use it?

Some tasks need deep focus. When you ask Claude to research something, review code, AND write a summary all at once, it juggles too many things in one conversation. Agents let you split work into separate focused workers. Each one does its job and reports back.

This also keeps your main conversation clean. The agent does its work in the background, and you only see the result.

## How to set it up

Create the agents folder in your project:

```bash
mkdir -p .claude/agents
```

Create a markdown file for each agent:

```bash
touch .claude/agents/my-agent.md
```

Define the agent's role, tools, and instructions inside that file. Claude will use this agent when you mention it or when the task matches.

## Example 1: For Everyone

You often need to research topics for work or school. Instead of asking Claude to do everything at once, create a dedicated research agent.

Create this file at `.claude/agents/research-assistant.md`:

```markdown
---
name: research-assistant
description: Researches a topic and writes a clear summary with sources
model: sonnet
tools:
  - WebSearch
  - WebFetch
  - Read
  - Write
---

# Research Assistant

You are a research assistant. When given a topic, you:

1. Search the web for reliable, recent information
2. Read the top 3-5 sources
3. Write a clear summary in plain English (no jargon)
4. List your sources at the bottom

## Rules

- Keep summaries under 500 words
- Use bullet points for key facts
- Always include at least 3 sources
- If you find conflicting information, mention both sides
- Save the summary to a file called research-[topic].md
```

Now in Claude Code, you can say:

```
use the research-assistant agent to look up "how solar panels work"
```

The agent searches the web, reads sources, and writes a clean summary file. Your main conversation stays uncluttered.

**Without agents:** Claude tries to research, summarize, and stay in your conversation all at once. Long conversations get messy.

**With agents:** The research agent works separately, saves a file, and you keep your main session focused.

## Example 2: For Developers

You want code reviews before committing, but don't want to clutter your main session with review comments.

Create this file at `.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: Reviews code changes for bugs, style issues, and potential problems
model: opus
tools:
  - Read
  - Glob
  - Grep
  - Bash
---

# Code Reviewer

You review code changes. When asked, you:

1. Run `git diff` to see what changed
2. Read each modified file in full for context
3. Check for:
   - Bugs or logic errors
   - Missing error handling
   - Security issues (SQL injection, XSS, exposed secrets)
   - Code that doesn't match existing patterns in the codebase
4. Give feedback as a numbered list, sorted by severity

## Feedback format

For each issue found:
- **Severity**: critical / warning / suggestion
- **File**: path and line number
- **Issue**: what's wrong
- **Fix**: what to do instead

If the code looks good, say so. Don't invent problems.

## Rules

- Never modify files. Only read and report.
- Focus on real bugs, not style preferences.
- If you're unsure about something, say "I'm not sure" rather than guessing.
```

Now before committing, say:

```
use the code-reviewer agent to review my changes
```

The agent checks your diff, reads the full files, and gives a structured review without touching your code.

## Try it yourself

1. Create `.claude/agents/` in your project
2. Copy one of the examples from [`examples/`](examples/) into it
3. Open Claude Code and ask it to use your agent
4. See how the agent works in its own context and reports back

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- Name your agents by what they DO, not what they ARE: `code-reviewer` not `senior-engineer`
- Give agents specific tools. A research agent needs WebSearch. A code reviewer needs Read and Grep. Don't give every agent every tool.
- Use `model: sonnet` for fast, simple tasks. Use `model: opus` for complex reasoning. If omitted, it inherits the parent conversation's model.
- Say "use subagents" in your prompt to tell Claude to throw more compute at a problem.
- Agents keep your main conversation clean. If a task takes many steps, delegate it.
- Use `maxTurns` to limit how long an agent can run before stopping.
- Use `isolation: worktree` to run an agent in a separate git worktree so it can't affect your working files.
- Write the `description` field like a trigger -- it tells Claude WHEN to use this agent automatically.
