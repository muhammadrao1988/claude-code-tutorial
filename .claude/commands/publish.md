---
description: Publish a completed tutorial - update README, create spec, commit, push
---

# Publish Tutorial

Publish a completed tutorial to GitHub. Ask me which tutorial number to publish (e.g., 01, 02).

Then do these steps in order:

1. **Verify the tutorial is complete**
   - Check that README.md exists and has content (not just a placeholder)
   - Check that examples/non-tech/ has at least one file
   - Check that examples/dev/ has at least one file
   - Check that try-it-yourself.md has content
   - If anything is missing, tell me what's incomplete and stop

2. **Update root README.md**
   - Find the tutorial's row in the table
   - Change "Coming soon" to "Available"

3. **Create spec**
   - Use the spec-tracker agent to create a spec in `specs/`

4. **Move backlog item**
   - Move the topic from `backlog/pending/` to `backlog/completed/`

5. **Commit and push**
   - Stage all changes related to this tutorial
   - Commit with message: "add tutorial [number]: [topic]"
   - Push to origin main

6. **Confirm**
   - Show the GitHub URL for the new tutorial
