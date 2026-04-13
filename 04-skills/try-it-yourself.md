# Try it yourself: Skills

## Exercise

Create a skill that enforces a standard you care about.

### Step 1: Pick a standard

Think of rules you repeat to Claude:
- "Always use this writing tone"
- "Follow these naming conventions"
- "Structure documents this way"
- "Check these things before finishing"

### Step 2: Create the skill folder

```bash
cd /path/to/your/project
mkdir -p .claude/skills/my-standard
```

### Step 3: Write the SKILL.md

```bash
touch .claude/skills/my-standard/SKILL.md
```

Add frontmatter and instructions:

```markdown
---
name: my-standard
description: Use when [describe the trigger situation]
---

# My Standard

[Write your rules here in clear bullet points]
```

### Step 4: Test automatic loading

Open Claude Code and start a task that matches your skill's description. You should see Claude mention loading the skill.

### Step 5: Test manual invocation

Type `/my-standard` in Claude Code. The skill loads immediately, regardless of context.

### Bonus: Add supporting files

Create an `examples/` subfolder with a sample that shows what "good" looks like:

```bash
mkdir -p .claude/skills/my-standard/examples
touch .claude/skills/my-standard/examples/good-example.md
```

Claude can reference these examples when applying your skill.
