# Skills

## What is it?

Skills are knowledge modules that Claude loads automatically when relevant. You create a skill as a folder with a `SKILL.md` file inside it. When Claude detects that your task matches a skill's description, it loads the instructions and follows them.

Think of skills as cheat sheets. You write down how something should be done once, and Claude pulls out the right cheat sheet at the right time.

## Why use it?

You probably have rules or patterns you want Claude to follow every time -- a writing style, a coding convention, a review checklist. Without skills, you repeat these instructions in every conversation. With skills, Claude automatically applies them when the situation fits.

Skills are different from CLAUDE.md (which is always loaded) and commands (which you invoke manually). Skills sit in between: Claude discovers and loads them on its own when needed.

## How to set it up

Create a skill folder with a `SKILL.md` file inside:

```bash
mkdir -p .claude/skills/my-skill
touch .claude/skills/my-skill/SKILL.md
```

The `SKILL.md` file has two parts: frontmatter (settings) and content (instructions).

```markdown
---
name: my-skill
description: When Claude should use this skill
---

Your instructions here...
```

The `description` field is the trigger. Write it for Claude, not for humans. It should answer: "When should I load this?"

## Example 1: For Everyone

You write a lot of emails and want Claude to always follow a professional style.

Create this folder and file at `.claude/skills/email-writer/SKILL.md`:

```markdown
---
name: email-writer
description: Use when drafting, editing, or reviewing any email
---

# Email Writing Style

When writing or editing emails, follow these rules:

## Structure
1. Start with a clear subject line
2. Open with a greeting and one sentence of context
3. State your main point or request in the first paragraph
4. Use short paragraphs (2-3 sentences each)
5. End with a clear next step or call to action
6. Close with an appropriate sign-off

## Tone
- Professional but warm, not robotic
- Use "I" and "you" naturally
- Avoid jargon unless the recipient would expect it
- Never use "per my last email" or passive-aggressive language

## Length
- Keep emails under 150 words when possible
- If it needs to be longer, use bullet points
- If it needs more than 300 words, suggest splitting into multiple emails

## Before sending
- Check: Is the ask clear? Would you know what to do if you received this?
- Check: Is the tone appropriate for the relationship?
- Check: Are there any typos or awkward phrases?
```

Now when you say "help me write an email to my boss about the delayed project," Claude automatically loads this skill and follows your email style rules.

You can also invoke it directly by typing `/email-writer` in Claude Code.

## Example 2: For Developers

You want to enforce REST API naming conventions across your team's codebase.

Create this folder and file at `.claude/skills/api-design/SKILL.md`:

```markdown
---
name: api-design
description: Use when creating, modifying, or reviewing REST API endpoints, routes, or schemas
---

# REST API Design Conventions

Follow these conventions for all API work in this project.

## URL Patterns
- Use plural nouns: `/users`, `/orders`, `/products`
- Use kebab-case for multi-word resources: `/order-items`
- Nest for relationships: `/users/{id}/orders`
- Never use verbs in URLs: `/users` not `/getUsers`
- Version in the URL: `/api/v1/users`

## HTTP Methods
- GET: read (never changes data)
- POST: create new resource
- PUT: replace entire resource
- PATCH: update specific fields
- DELETE: remove resource

## Response Format
- Always return JSON
- Wrap responses: `{ "data": ..., "meta": { "total": N } }`
- Errors: `{ "error": { "code": "NOT_FOUND", "message": "..." } }`
- Use HTTP status codes correctly (201 for created, 404 for not found)

## Naming
- camelCase for JSON fields: `firstName`, `createdAt`
- snake_case for query params: `?sort_by=name&page_size=20`
- Consistent date format: ISO 8601 (`2026-04-13T10:30:00Z`)
```

Now whenever you work on API endpoints -- creating, modifying, or reviewing -- Claude loads these conventions automatically.

## Try it yourself

1. Create `.claude/skills/my-skill/` in your project
2. Copy one of the examples from [`examples/`](examples/) into it
3. Open Claude Code and start a task that matches the skill's description
4. Notice how Claude loads and follows the skill without you asking

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- The `description` is a trigger, not a summary. Write it as: "Use when [doing X]."
- Add supporting files in subfolders like `examples/` or `scripts/` for complex skills. Claude discovers these through progressive disclosure.
- Set `disable-model-invocation: true` if you only want the skill to run when YOU type `/skill-name`, not when Claude decides to load it.
- Set `context: fork` to run the skill in an isolated subagent so it doesn't clutter your main conversation.
- Skills can include shell commands with `` !`command` `` syntax. Claude runs the command and sees the output when the skill loads.
- Keep skills focused on one thing. A "writing-style" skill and a "code-review" skill are better than one "do-everything" skill.
