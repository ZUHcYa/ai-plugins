---
name: "ideas"
description: "List all canvases in the ideate/ folder with progress status"
---

# /knvs:ideas

## Purpose

Shows all canvases in the `ideate/` folder with their progress status.

## When to Use

- When you want to see all your work-in-progress ideas at a glance
- Reviewing which ideas are ready for validation
- Getting an overview of your idea pipeline

## Workflow

```
User: /knvs:ideas
Claude: Open Ideas (ideate/):

        1. AI Bookkeeping for Freelancers [WIP]
           Created: 2026-01-24
           -> ideate/ai-bookkeeping.md

        2. Fitness App for Seniors [WIP]
           Created: 2026-01-20
           -> ideate/fitness-seniors.md

        3. B2B Invoice Tool [READY FOR EXPLORE]
           Created: 2026-01-15
           -> ideate/b2b-invoice.md
```

## What the Skill Does

1. Scans all files in `ideate/` folder
2. Shows title, progress, created date
3. Groups by progress (WIP first, then READY FOR EXPLORE)

## Output Format

Each idea shows:
- Title from the canvas
- Progress state (WIP or READY FOR EXPLORE)
- Created date
- File path

> **STRICT:** Show progress ONLY as `[WIP]` or `[READY FOR EXPLORE]`. No percentages, no sub-states, no other variants.

## Notes

- Ideas with READY FOR EXPLORE status are candidates for `/knvs:explore`
- Use this skill to decide which idea to work on next

