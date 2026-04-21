# Try it yourself: Learning Loop

## Exercise

Tune your real resume to a real job description.

### Step 1: Copy the example

```bash
cp -r examples/non-tech ~/my-resume-tuner
cd ~/my-resume-tuner
```

### Step 2: Add your real content

Open `input/resume.md` and paste your actual resume in markdown format.

Open `input/job-description.md` and paste a real job description you're interested in.

### Step 3: Initialize git

The loop uses git to remember what worked. This step is required.

```bash
git init
git add .
git commit -m "starting point"
```

### Step 4: Run the loop

```bash
claude
```

Inside Claude Code:

```
/tune
```

### What you'll see

1. Claude verifies your input files are filled in
2. The rubric-writer agent reads your JD and writes a custom rubric to `output/rubric.md`
3. Claude shows you the rubric and asks if you want to adjust
4. Type "go" to continue
5. The tuner runs 15 iterations -- each one rewrites a section, scores, and commits or reverts
6. You get a summary at the end

### When it finishes

```bash
cat output/resume-tuned.md         # see your tuned resume
cat output/score-history.md        # see what changed each round
git log --oneline                  # see only the winning iterations
```

### For the next job

Edit `input/job-description.md` with a new JD. Delete `output/`. Run `/tune` again. Your original `input/resume.md` stays the same across all jobs.

### Bonus: try the dev example

Same pattern but for tuning a Claude skill:

```bash
cp -r examples/dev ~/my-skill-tuner
cd ~/my-skill-tuner
# put a skill in input/skill.md
git init && git add . && git commit -m "starting point"
claude
/tune
```
