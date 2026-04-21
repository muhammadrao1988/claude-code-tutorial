---
description: Tune the resume in input/ to match the job description using a learning loop
---

Run the full resume tuning pipeline.

## Pre-flight check

Verify these files exist and are not empty:
- `input/resume.md`
- `input/job-description.md`

If either is missing or empty, tell me what to add and stop.

## Step 1: Generate the rubric

Use the rubric-writer agent to read `input/job-description.md` and write a tailored rubric to `output/rubric.md`.

Show me the rubric and ask if I want to adjust anything before the loop starts.

## Step 2: Run the loop

Use the tuner agent to run 15 iterations.

## Step 3: Summarize

Show me:
- Starting vs final score
- The final tuned resume (output/resume-tuned.md)
- Top 3 changes that moved the score the most
- `git log --oneline` of winning iterations
