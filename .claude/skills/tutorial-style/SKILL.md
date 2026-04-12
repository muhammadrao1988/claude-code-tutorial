---
name: tutorial-style
description: Use when writing or reviewing tutorial content to enforce consistent style and structure
---

# Tutorial Writing Style Guide

## Structure

Every tutorial README.md follows this exact structure:

1. **What is it?** -- One paragraph, plain English
2. **Why use it?** -- Real-world problem it solves
3. **How to set it up** -- Step-by-step with code blocks
4. **Example 1: For Everyone** -- Non-technical walkthrough
5. **Example 2: For Developers** -- Programming walkthrough
6. **Try it yourself** -- Points to examples/ folder

## Language Rules

- Plain English. No jargon unless you explain it immediately.
- Short paragraphs: 2-3 sentences maximum.
- Use "you" to address the reader directly.
- No emojis anywhere in tutorial content.
- Explain the "what happens" not just the "how to set up."

## Code Rules

- Every code block must be copy-pasteable. No placeholder paths.
- Show the exact file path where each file should be created.
- Use bash code blocks for terminal commands.
- Use markdown code blocks for file contents.
- Always show the complete file, not snippets.

## Length

- Each tutorial README: under 200 lines.
- Each try-it-yourself.md: under 50 lines.
- Backlog items: under 10 lines.
- Spec files: under 30 lines.

## Quality Checks

Before considering a tutorial complete:
- Can a reader copy every code block and have it work?
- Does it have BOTH a non-tech and dev example?
- Would a non-programmer understand the "For Everyone" section?
- Would a developer find the "For Developers" section useful?
