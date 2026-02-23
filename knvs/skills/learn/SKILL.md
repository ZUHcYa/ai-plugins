---
name: "learn"
description: "Extract insights from completed experiment results"
---

# /knvs:learn

## Purpose

Extracts key learnings (insights) from completed experiments and updates
the corresponding hypothesis confidence level.
**Only applicable to EXPLORE canvases.** EXPLOIT canvases do not use the hypothesis loop.

## When to Use

- After completing an experiment and documenting results
- When you want to distill learnings from experiment data
- Before making a Persevere/Pivot/Kill decision
- **NOT for EXPLOIT canvases** — these have their own mechanisms (Performance & Trend Assessments)

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
        Status: testing → validated
        Result: SUCCESS (82% > 70% target)

        Confidence assessment:
        Completed experiments: 1 (Online Survey, evidence_strength: strong)
        Recommended confidence: MODERATE
        (1 experiment with strong evidence — but only one experiment so far)

        -> Accept? [Y/n/edit]

User: Y
Claude: Confidence: — → moderate
        Updated: hypotheses/ai-bookkeeping/receipt-categorization-need.md

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Insights created: 2
        Hypothesis updated: receipt-categorization-need (VALIDATED, MODERATE)

        Remaining hypotheses for AI Bookkeeping:
        - ai-accuracy-requirement (open, HIGH, —) → needs experiment
        - customers-will-pay-monthly (testing, MEDIUM, VERY-LOW)
        - linkedin-channel-effectiveness (open, MEDIUM, —)
        - cloud-hosting-costs (open, LOW, —)

        Next steps:
        - /knvs:card [E] to create an Experiment Learning Card
        - /knvs:card [H] to conclude a hypothesis testing cycle
        - /knvs:experiment for untested hypotheses
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
   - Status: testing → validated/invalidated (based on success criteria)
   - Confidence level: assessed based on ALL completed experiments for this hypothesis
7. Updates experiment `result` field (success/failure/inconclusive)
8. Shows remaining hypothesis overview

## Status Update Logic

| Experiment Result | Status Update |
|-------------------|--------------|
| Success (criteria met) | status → validated |
| Partial success | status stays testing |
| Failure (criteria not met) | status → invalidated |
| Inconclusive | status stays testing |

## Confidence Level Update Logic

After updating the hypothesis status, the skill assesses the confidence level based on **all completed experiments** for this hypothesis (not just the current one).

**Assessment process:**
1. Collect all completed experiments linked to this hypothesis
2. Count total experiments, their `evidence_strength`, and `experiment_type`
3. Recommend confidence level based on the criteria below
4. User confirms or overrides

| Condition | Recommended Confidence |
|-----------|----------------------|
| 1 experiment, evidence_strength: weak | `very-low` |
| 1+ experiments, all evidence_strength: weak (Discovery/Say-data only) | `low` |
| Multiple experiments with at least 1x evidence_strength: strong, OR 1 strong CTA experiment | `moderate` |
| Multiple experiments, at least 1 Validation/CTA experiment with evidence_strength: strong | `high` |

**Experiment type classification:**

| Category | Types | Evidence Type |
|----------|-------|---------------|
| Discovery (Say) | customer-interview, online-survey, data-analysis, paper-prototype | Stated preferences |
| Validation (Do/CTA) | concierge-mvp, wizard-of-oz, single-feature-mvp, pre-sale, crowdfunding | Actual behavior |
| Borderline | landing-page | Depends on commitment level |

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
- Confidence levels are updated based on all completed experiments for the hypothesis
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
