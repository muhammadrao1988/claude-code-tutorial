---
description: Tune the skill in input/ to pass more test cases using a learning loop
---

Run the full skill tuning pipeline.

## Pre-flight check

Verify `input/skill.md` exists and is not empty. If missing, tell me to add it and stop.

## Step 1: Generate test cases

Use the test-writer agent to read `input/skill.md` and write test cases to `output/test-cases.md`.

Show me the test cases and ask if I want to add or remove any before the loop starts.

## Step 2: Run the loop

Use the tuner agent to run 20 iterations.

## Step 3: Summarize

Show me:
- Starting vs final pass rate
- The final tuned skill (output/skill-tuned.md)
- Top 3 changes that improved the skill the most
- `git log --oneline` of winning iterations
