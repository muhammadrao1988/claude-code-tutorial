# Slash Commands

## What is it?

Slash commands are reusable prompt templates you save as files. Instead of typing the same long instruction every time, you type `/command-name` and Claude runs your saved prompt.

They live in `.claude/commands/` inside your project. You create them once, use them forever.

## Why use it?

You probably ask Claude to do the same things over and over. "Summarize this folder." "Create a new component." "Review my changes." Each time, you retype or rephrase the instruction. Commands let you save that instruction once and reuse it with a single slash.

## How to set it up

Create the commands folder in your project:

```bash
mkdir -p .claude/commands
```

Create a markdown file for each command:

```bash
touch .claude/commands/my-command.md
```

Write your prompt inside that file. That's it. Next time you open Claude Code, type `/my-command` and it runs.

## Example 1: For Everyone

You have a folder full of notes -- meeting notes, ideas, research. You want a quick way to get a summary of everything.

Create this file at `.claude/commands/organize-notes.md`:

```markdown
---
description: Summarize and organize all notes in the current folder
---

Look at all the .txt and .md files in this folder.

For each file:
1. Read the contents
2. Write a one-sentence summary

Then group the summaries by topic and show them in a list like this:

**Topic Name**
- file1.md: summary here
- file2.txt: summary here

At the end, suggest which notes could be merged or deleted.
```

Now open Claude Code in your notes folder and type:

```
/organize-notes
```

Claude reads all your notes, groups them by topic, and suggests cleanup. No need to explain what you want every time.

**Before commands:** "Hey Claude, can you look at all the files in this folder and summarize them? Group them by topic. Also tell me which ones I can delete."

**After commands:** `/organize-notes`

## Example 2: For Developers

You build REST APIs and always follow the same steps to add a new endpoint. Create a command that scaffolds everything at once.

Create this file at `.claude/commands/create-endpoint.md`:

```markdown
---
description: Scaffold a new REST API endpoint with router, schema, and test
---

I need a new API endpoint. Ask me:
1. What resource? (e.g., products, users, orders)
2. What operation? (e.g., list, create, update, delete)

Then create these files following the existing patterns in the codebase:

1. **Router** in `app/routers/` -- the endpoint handler
2. **Schema** in `app/schemas/` -- Pydantic request/response models
3. **Test** in `tests/` -- at least 2 test cases (success + error)

Follow the patterns you see in existing router and schema files. Run the tests after creating them.
```

Now instead of explaining your project structure every time:

```
/create-endpoint
```

Claude asks what you need, then creates the router, schema, and test files matching your existing code patterns.

## Try it yourself

1. Create `.claude/commands/` in any project folder
2. Copy one of the examples from [`examples/`](examples/) into it
3. Open Claude Code and type `/` to see your command listed
4. Run it and see what happens

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- The file name becomes the command name: `review-pr.md` becomes `/review-pr`
- Add a `description` in the frontmatter so you remember what each command does
- Start simple. A 5-line command is better than no command.
- Commands can ask you questions using the `$ARGUMENTS` placeholder
- Share commands with your team by committing the `.claude/commands/` folder to git (if you want to)
