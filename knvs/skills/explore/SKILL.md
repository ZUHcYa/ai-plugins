---
name: "explore"
description: "Move canvas to EXPLORE phase for hypothesis-driven validation"
---

# /knvs:explore

## Purpose

Moves a canvas from `ideate/` to `explore/` to begin the validation loop.

## When to Use

- When your idea research is complete and you're ready to validate assumptions
- Transitioning from IDEATE to EXPLORE phase

## Workflow

```
User: /knvs:explore
Claude: Which idea do you want to explore?

        1. AI Bookkeeping for Freelancers [READY FOR EXPLORE]
        2. Fitness App for Seniors [WIP]

User: 1
Claude: Canvas moved: ideate/ai-bookkeeping.md -> explore/ai-bookkeeping.md
        Status: EXPLORE

        Next step: Run /knvs:hypothesize to extract and prioritize
        hypotheses from your Business Model Canvas.
```

## What the Skill Does

1. Lists all canvases in `ideate/` folder (prioritizes READY FOR EXPLORE)
2. User selects which idea to explore
3. **Moves file from `ideate/` to `explore/`**
4. Updates canvas status to EXPLORE
5. Removes `progress` field (only used in IDEATE phase)
6. Adds `innovation_risk` and `potential_revenue` fields
7. Points user to `/knvs:hypothesize` as the next step

## Notes

- File is physically moved from `ideate/` to `explore/`
- Status in frontmatter changes from IDEATE to EXPLORE
- Progress field is removed (only used in IDEATE phase)
- Creates `hypotheses/<canvas-slug>/` folder for future hypotheses

## Next Steps

After moving to EXPLORE:
1. Run `/knvs:hypothesize` to extract hypotheses from your BMC
2. Prioritize hypotheses (importance × evidence)
3. Design experiments for high-priority hypotheses with `/knvs:experiment`
4. Run experiments and extract insights with `/knvs:learn`
5. Make decisions with `/knvs:decide` (Persevere / Pivot / Kill)

---

## Canvas Template

Canonical template for EXPLORE canvases:

```markdown
---
status: EXPLORE
created: YYYY-MM-DD
updated: YYYY-MM-DD
innovation_risk: MEDIUM # LOW | MEDIUM | HIGH
potential_revenue: 5 # 1-10
---

# Business Model Canvas: [Name]

## Value Proposition

## Customer Segments

## Channels

## Customer Relationships

## Revenue Streams

## Key Resources

## Key Activities

## Key Partnerships

## Cost Structure

---

## Decisions
<!-- Decision log entries will be added here by /knvs:decide -->

## Next Steps
- [ ]
```
