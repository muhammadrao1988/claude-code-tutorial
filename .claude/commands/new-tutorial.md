---
description: Scaffold a new tutorial folder with all required files
---

# New Tutorial

Create a new tutorial. Ask me these questions one at a time:

1. What number is this tutorial? (e.g., 01, 02, 03)
2. What is the topic? (e.g., memory, commands, agents)
3. What is the non-tech example? (a real-life scenario for non-programmers)
4. What is the dev example? (a programming scenario for developers)

After I answer, create this folder structure:

```
[number]-[topic]/
├── README.md
├── examples/
│   ├── non-tech/
│   │   └── .gitkeep
│   └── dev/
│       └── .gitkeep
└── try-it-yourself.md
```

Put a placeholder in README.md with the tutorial template from CLAUDE.md.
Put a placeholder in try-it-yourself.md explaining what exercise will go here.

Then update `backlog/pending/` to mark this topic as "in-progress" if a backlog item exists for it.

Finally, tell me the folder is ready and suggest I use the tutorial-writer agent to fill in the content.
