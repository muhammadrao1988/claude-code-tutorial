# Memory (CLAUDE.md)

## What is it?

CLAUDE.md is a file you put at the root of your project. Claude reads it at the start of every conversation. It tells Claude who you are, what your project is about, and how you like to work.

Think of it as a sticky note on your desk that Claude checks before doing anything.

## Why use it?

Without CLAUDE.md, Claude starts every conversation from scratch. You have to re-explain your project, your preferences, and your rules every time. With CLAUDE.md, Claude already knows the context before you even ask your first question.

## How to set it up

Create a file called `CLAUDE.md` in your project's root folder:

```bash
touch CLAUDE.md
```

Open it and write instructions in plain English. That's it. No special syntax needed.

Here are two complete examples.

## Example 1: For Everyone

You're a **teacher** preparing lesson plans. You want Claude to always keep language simple and age-appropriate.

Create this `CLAUDE.md` in your lesson plans folder:

```markdown
# My Lesson Plans

I am a 5th grade science teacher.

## Rules

- Use simple words a 10-year-old can understand
- Keep explanations under 3 sentences
- Always include a real-world example when explaining a concept
- When I ask you to review my notes, check for jargon and suggest simpler alternatives

## My current topics

- Solar system (this month)
- Weather and climate (next month)
```

Now when you open Claude Code in that folder and say "help me write a lesson about why it rains," Claude already knows to use simple language and include a real-world example.

**What happens without CLAUDE.md:** Claude might write a college-level explanation with terms like "precipitation" and "condensation nuclei."

**What happens with CLAUDE.md:** Claude writes something like "Rain happens when tiny water drops in clouds get heavy enough to fall. Think of it like filling a sponge with water -- eventually it gets too heavy and drips."

## Example 2: For Developers

You're working on a **Python project** with specific tools and conventions.

Create this `CLAUDE.md` in your project root:

```markdown
# Inventory API

A REST API for managing warehouse inventory, built with FastAPI and PostgreSQL.

## Tech Stack

- Python 3.12, FastAPI, SQLAlchemy
- PostgreSQL 16 with Alembic migrations
- pytest for testing

## Commands

- Run server: `uvicorn app.main:app --reload`
- Run tests: `pytest -v`
- Run linter: `ruff check .`
- New migration: `alembic revision --autogenerate -m "description"`

## Rules

- Always run `pytest -v` before suggesting a commit
- Use type hints on all function signatures
- Follow existing patterns in app/routers/ for new endpoints
- Never modify alembic/versions/ files directly
```

Now when you say "add an endpoint for deleting products," Claude knows to use FastAPI, follow your router patterns, add type hints, and run tests before committing.

## Try it yourself

1. Go to any folder where you use Claude Code
2. Copy one of the examples from [`examples/`](examples/) into that folder
3. Rename it to `CLAUDE.md`
4. Edit it to match your actual project or work
5. Start a new Claude Code session and notice how Claude already knows your context

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- Keep it under 200 lines. Long files get partially ignored.
- Put the most important rules first.
- Use bullet points, not paragraphs.
- Update it when your project changes.
- You can have multiple CLAUDE.md files in subdirectories for different parts of a project.
