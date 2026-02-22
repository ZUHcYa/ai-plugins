---
name: "learn"
description: "Extract insights from completed experiment results"
---

# /knvs:learn

## Purpose

Extracts key learnings (insights) from completed experiments and updates
the corresponding hypothesis evidence level.
**Only applicable to EXPLORE canvases.** EXPLOIT canvases do not use the hypothesis loop.

## When to Use

- After completing an experiment and documenting results
- When you want to distill learnings from experiment data
- Before making a Persevere/Pivot/Kill decision
- **NOT for EXPLOIT canvases** — these have their own mechanisms (Disruption Reviews)

## Workflow

```
User: /knvs:learn
Claude: Which canvas has completed experiments?

        1. AI Bookkeeping (explore/) - 1 completed experiment
        2. Fitness App (explore/) - 0 completed experiments

User: 1
Claude: Completed experiments for AI Bookkeeping:

        1. Receipt Categorization Needs Survey [COMPLETED]
           Hypothesis: receipt-categorization-need
           -> experiments/ai-bookkeeping/receipt-categorization-survey.md

        Select experiment to extract insights from:

User: 1
Claude: Reading experiment results...

        Experiment: Receipt Categorization Needs Survey
        Result: 82% of respondents identified receipt categorization
                as a top-3 pain point (target: >70%)

        I've identified 2 key insights:

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        INSIGHT 1 of 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Title: "Receipt categorization is a validated pain point"
        BMC Fields: Value Proposition
        Tags: [validation, customer-need]

        Learning: 82% of surveyed freelancers rank receipt
        categorization as a top-3 bookkeeping pain point.

        -> Accept? [Y/n/edit]

User: Y
Claude: Saved: insights/ai-bookkeeping/receipt-categorization-validated.md

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        INSIGHT 2 of 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Title: "Mobile upload is the expected input method"
        BMC Fields: Channels, Key Activities
        Tags: [ux, mobile]

        Learning: 68% of respondents expect to photograph receipts
        with their phone. Desktop upload is secondary.

        -> Accept? [Y/n/edit]

User: Y
Claude: Saved: insights/ai-bookkeeping/mobile-upload-expected.md

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS UPDATE
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Hypothesis: receipt-categorization-need
        Evidence: none → strong
        Status: testing → validated
        Result: SUCCESS (82% > 70% target)

        Updated: hypotheses/ai-bookkeeping/receipt-categorization-need.md

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Insights created: 2
        Hypothesis updated: receipt-categorization-need (VALIDATED)

        Remaining hypotheses for AI Bookkeeping:
        - ai-accuracy-requirement (open, HIGH, NONE) → needs experiment
        - customers-will-pay-monthly (testing, MEDIUM, WEAK)
        - linkedin-channel-effectiveness (open, MEDIUM, NONE)
        - cloud-hosting-costs (open, LOW, NONE)

        Next steps:
        - /knvs:experiment for untested hypotheses
        - /knvs:decide when ready for a decision
```

## What the Skill Does

1. Lists canvases in `explore/` with completed experiments
2. User selects canvas, then completed experiment
3. Reads experiment results and conclusion
4. Identifies key learnings from the results
5. For each insight:
   - Proposes title, BMC fields, tags, learning
   - User confirms, rejects, or edits
   - Creates insight file in `insights/<canvas-slug>/`
6. Updates the corresponding hypothesis:
   - Evidence level: none → weak → strong (based on experiment result)
   - Status: testing → validated/invalidated (based on success criteria)
7. Updates experiment `result` field (success/failure/inconclusive)
8. Shows remaining hypothesis overview

## Evidence Level Update Logic

| Experiment Result | Evidence Update |
|-------------------|-----------------|
| Success (criteria met) | evidence → strong, status → validated |
| Partial success | evidence → weak, status stays testing |
| Failure (criteria not met) | evidence → strong, status → invalidated |
| Inconclusive | evidence → weak, status stays testing |

## Edge Cases

### Experiment has no results section filled

```
Claude: Experiment "Receipt Categorization Survey" has no results documented.

        Please fill in the ## Results and ## Conclusion sections
        in experiments/ai-bookkeeping/receipt-categorization-survey.md
        before running /knvs:learn.
```

### No completed experiments

```
Claude: No completed experiments found for any EXPLORE canvas.

        Run experiments first, document results, then use /knvs:learn.
```

## Notes

- Insights are individual .md files in `insights/<canvas-slug>/`
- Each insight is standalone and reusable across canvas boundaries
- The skill reads experiment results — it does NOT generate fake data
- Evidence levels are updated based on actual experiment outcomes
- Insights can also be created manually in any Markdown editor

---

## Insight Template

Canonical template (also in CLAUDE.md):

```markdown
---
type: insight
title: "[Key learning]"
source_experiment: experiments/canvas-slug/experiment-slug.md
canvas: explore/canvas-slug.md
bmc_fields:
  - Value Proposition
created: YYYY-MM-DD
tags: [tag1, tag2]
---

# Insight: [Title]

## Learning

[What we learned from the experiment]

## Evidence

[Data and observations that support this learning]

## Implications

[How this affects the business model]

## Source

[[experiments/canvas-slug/experiment-slug.md]]
```
