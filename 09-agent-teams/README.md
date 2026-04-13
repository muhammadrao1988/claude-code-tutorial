# Agent Teams

## What is it?

Agent Teams let you run **multiple Claude Code sessions** that work together on the same task. One session acts as the team lead, and the others are teammates. They share a task list, communicate with each other, and coordinate their work.

This is different from subagents (Tutorial 03). Subagents run inside your session and report back. Agent teams are separate sessions that talk to each other directly.

## Why use it?

Some tasks are too big or too complex for a single Claude session. Reviewing a pull request from three different angles. Investigating a bug with competing theories. Building multiple modules at the same time. Agent teams let you throw parallel compute at these problems -- each teammate works independently while coordinating through shared tasks.

## Subagents vs Agent Teams

Before setting up a team, make sure you need one. Here's when to use each:

| | Subagents (Tutorial 03) | Agent Teams |
|---|---|---|
| **What it is** | Worker inside your session | Separate Claude session |
| **Communication** | Reports back to you only | Teammates talk to each other |
| **Coordination** | You manage everything | Shared task list, self-organized |
| **Context** | Shares your session context | Own context window |
| **Best for** | Focused tasks (research, review) | Complex parallel work (multi-angle review, competing hypotheses) |
| **Token cost** | Lower | Higher (each teammate is a full session) |
| **Setup** | Just create `.claude/agents/` | Must enable in settings.json |

**Rule of thumb:** If teammates don't need to talk to each other, use subagents. If they need to share findings, challenge each other, or coordinate, use agent teams.

## How to set it up

Agent teams are experimental and disabled by default. You must enable them first.

### Step 1: Enable in settings.json

Add this to your `.claude/settings.json` or `~/.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

This is required. Without it, Claude cannot create teams.

### Step 2: Start a team

Open Claude Code and describe the task and team structure:

```
Create an agent team with 3 teammates to [your task here]
```

Claude creates the team, spawns teammates, assigns tasks, and coordinates.

### Step 3: Interact with teammates

- **In-process mode** (default): press `Shift+Down` to cycle through teammates. Type to message them directly.
- **Split-pane mode** (tmux/iTerm2): each teammate gets its own terminal pane. Click to interact.

To use split panes, set this in `~/.claude.json`:

```json
{
  "teammateMode": "tmux"
}
```

Or pass it as a flag:

```bash
claude --teammate-mode tmux
```

## Example 1: For Everyone

You're **planning an event** (birthday party, team offsite, conference) and want to explore multiple aspects at once.

First, enable agent teams in your settings (see Step 1 above).

Then in Claude Code:

```
Create an agent team to help plan a company team offsite for 20 people.
Spawn 3 teammates:
- One focused on venue and logistics (location, dates, travel)
- One focused on activities and agenda (team building, workshops, meals)
- One playing devil's advocate (budget concerns, what could go wrong, backup plans)

Have them research and share findings with each other.
```

Claude creates the team. Each teammate investigates their area, then they share findings. The logistics teammate might discover a venue that the activities teammate can plan around. The devil's advocate challenges assumptions like "is the venue accessible to everyone?"

**Without agent teams:** You ask Claude one question at a time, switching between topics. It loses context and can't explore multiple angles simultaneously.

**With agent teams:** Three teammates research in parallel, challenge each other, and produce a more thorough plan.

## Example 2: For Developers

You want a **parallel code review** where different reviewers focus on different concerns.

Enable agent teams (Step 1), then:

```
Create an agent team to review the changes in this branch.
Spawn 3 teammates:
- One focused on security (auth, input validation, exposed secrets)
- One focused on performance (queries, loops, memory usage)
- One focused on test coverage (missing tests, edge cases, mocking)

Have them each review and report findings. They should challenge
each other's findings if they disagree.
```

Each reviewer works from the same codebase but applies a different lens. When they finish, the lead synthesizes findings across all three.

You can also use subagent definitions as teammates:

```
Spawn a teammate using the code-reviewer agent type to audit the auth module.
```

This uses the agent definition from `.claude/agents/code-reviewer.md` (Tutorial 03) but runs it as a full teammate instead of a subagent.

### Debugging with competing hypotheses

Another powerful pattern -- have teammates test different theories:

```
Users report the app crashes after login. Spawn 4 teammates to
investigate different hypotheses:
- Token expiration issue
- Database connection timeout
- Memory leak in session handling
- Race condition in auth middleware

Have them talk to each other to disprove each other's theories.
```

Each teammate investigates independently, then they debate. The theory that survives the challenge is most likely the real cause.

## Try it yourself

1. Enable agent teams in your settings (see Step 1)
2. Copy the settings from [`examples/`](examples/) if needed
3. Start Claude Code and ask it to create a team for a task you're working on
4. Watch the teammates coordinate and share findings

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- **Start small:** 3 teammates is the sweet spot. More teammates means more token cost and coordination overhead.
- **Give context:** Teammates don't inherit your conversation history. Include task details in your spawn prompt.
- **Avoid file conflicts:** Don't have two teammates edit the same file. Break work so each owns different files.
- **Wait for teammates:** If the lead starts doing work itself, tell it: "Wait for your teammates to complete their tasks before proceeding."
- **Clean up:** When done, tell the lead to "clean up the team." Shut down teammates first.
- **Require plan approval:** For risky tasks, say "require plan approval before they make changes" -- the lead reviews each teammate's plan before they implement.
- **5-6 tasks per teammate** keeps everyone productive without too much context switching.
- Agent teams use significantly more tokens. For simple tasks, a single session or subagents are more cost-effective.
