# Try it yourself: Orchestration

## Exercise

Build your own orchestrated workflow by combining a command, agent, and skill.

### Step 1: Pick a workflow

Think of a multi-step task you do regularly:
- "Review and organize my inbox"
- "Onboard to a new codebase"
- "Write and publish a blog post"
- "Debug and fix a reported bug"

### Step 2: Create the skill (quality standards)

What rules should always be followed? Create the skill first:

```bash
mkdir -p .claude/skills/my-workflow
touch .claude/skills/my-workflow/SKILL.md
```

Write your quality standards in SKILL.md.

### Step 3: Create the agent (worker)

What steps does the work involve? Create the agent:

```bash
mkdir -p .claude/agents
touch .claude/agents/my-worker.md
```

Define the agent's tools and step-by-step instructions.

### Step 4: Create the command (entry point)

How should the workflow start? Create the command:

```bash
mkdir -p .claude/commands
touch .claude/commands/my-workflow.md
```

Write a prompt that ties the agent and skill together.

### Step 5: Run it

```bash
claude
/my-workflow
```

### Step 6: Iterate

The first version won't be perfect. Run it a few times, notice where it breaks or produces weak results, and improve each piece.

The skill usually needs the most tuning -- add rules based on what goes wrong.
