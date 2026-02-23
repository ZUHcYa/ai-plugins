---
name: "card"
description: "Create a Learning Card from a completed experiment"
---

# /knvs:card

## Purpose

Creates a standalone Learning Card that synthesizes hypothesis, experiment results,
and insights into the classic "Testing Business Ideas" 4-part narrative.
**Only applicable to EXPLORE canvases.**

## When to Use

- After `/knvs:learn` — when insights exist for a completed experiment
- To create a compact summary of what was tested and learned
- Before or after `/knvs:decide` — the card works independently
- **NOT for EXPLOIT canvases**

## Workflow

```
User: /knvs:card
Claude: Which canvas has completed experiments with insights?

        1. AI Bookkeeping (explore/) - 2 experiments with insights
        2. Fitness App (explore/) - 1 experiment with insights

User: 1
Claude: Experiments with insights for AI Bookkeeping:

        1. Receipt Categorization Needs Survey [COMPLETED, SUCCESS]
           Hypothesis: receipt-categorization-need
           Insights: 2
           -> No learning card yet

        2. Pricing Survey with 50 Freelancers [COMPLETED, SUCCESS]
           Hypothesis: customers-will-pay-monthly
           Insights: 1
           -> No learning card yet

        Select experiment:

User: 1
Claude: Reading hypothesis, experiment results, and insights...

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        LEARNING CARD DRAFT
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        ## We believed that...

        Freelancers need automated receipt categorization as a core
        bookkeeping pain point.

        -> Accept? [Y/n/edit]

User: Y
Claude: ## We observed...

        82 of 100 respondents (82%) ranked receipt categorization
        as a top-3 pain point. 45% ranked it #1. Strongest signal
        among freelancers with revenue > EUR 50k.
        The survey exceeded the 70% success threshold.

        -> Accept? [Y/n/edit]

User: Y
Claude: ## From that we learned...

        Receipt categorization is a validated pain point — 82% of
        surveyed freelancers confirm this need. Additionally, mobile
        upload is the expected input method (68% expect phone-based
        receipt capture).

        -> Accept? [Y/n/edit]

User: Y
Claude: ## Therefore we will...

        What action follows from this learning?

User: Prioritize mobile-first receipt capture in our MVP.
      Desktop upload becomes a secondary feature.

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        LEARNING CARD SAVED
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        File: learning-cards/ai-bookkeeping/receipt-categorization-survey.md

        Remaining experiments without learning cards:
        - Pricing Survey with 50 Freelancers → /knvs:card

        Next steps:
        - /knvs:card for remaining experiments
        - /knvs:decide when ready for a decision
```

## What the Skill Does

1. Lists canvases in `explore/` with completed experiments that have insights
2. User selects canvas, then experiment
3. Reads source artifacts:
   - **Hypothesis** → `## Claim` section for "We believed that..."
   - **Experiment** → `## Results` and `## Conclusion` for "We observed..."
   - **Insight(s)** → `## Learning` and `## Evidence` for "From that we learned..."
4. Generates draft for first 3 sections from source data
5. User confirms, rejects, or edits each section
6. "Therefore we will..." → User provides free text
7. Creates learning card file in `learning-cards/<canvas-slug>/`
8. Shows remaining experiments without cards

## Multiple Insights per Experiment

When an experiment has multiple insights, "From that we learned..." synthesizes
all of them into a coherent narrative:

```
Claude: This experiment produced 2 insights. Combining them:

        ## From that we learned...

        Receipt categorization is a validated pain point (82%
        confirmation). Furthermore, mobile upload is the expected
        input method — 68% of respondents expect phone-based
        capture. Desktop upload is secondary.

        -> Accept? [Y/n/edit]
```

All insight file paths are listed in the `insights` frontmatter array.

## Edge Cases

### Experiment has no insights yet

```
Claude: Experiment "Receipt Categorization Survey" has no insights.

        Run /knvs:learn first to extract insights from this experiment.
```

### Learning card already exists

```
Claude: Learning card already exists:
        learning-cards/ai-bookkeeping/receipt-categorization-survey.md

        Overwrite? [Y/n]
```

### No completed experiments with insights

```
Claude: No completed experiments with insights found for any EXPLORE canvas.

        Complete experiments and extract insights first:
        - /knvs:capture to document experiment results
        - /knvs:learn to extract insights
```

## Notes

- One learning card per experiment (1:1 relationship)
- Filename matches the experiment slug
- The first 3 sections are pre-filled from source artifacts — the user confirms or edits
- "Therefore we will..." is always user-provided (never auto-generated)
- Learning cards are standalone and can be created manually in any Markdown editor
- The skill reads existing artifacts — it does NOT generate fake data

---

## Learning Card Template

Canonical template (also in CLAUDE.md):

```markdown
---
type: learning-card
title: "[Experiment title]"
canvas: explore/canvas-slug.md
hypothesis: hypotheses/canvas-slug/hypothesis-slug.md
experiment: experiments/canvas-slug/experiment-slug.md
insights:
  - insights/canvas-slug/insight-1.md
  - insights/canvas-slug/insight-2.md
result: success | failure | inconclusive
created: YYYY-MM-DD
---

# Learning Card: [Experiment Title]

## We believed that...

[Drawn from hypothesis claim — the testable assumption]

## We observed...

[Drawn from experiment results and conclusion — what actually happened]

## From that we learned...

[Synthesized from insight(s) — key learning with supporting evidence]

## Therefore we will...

[User input — next action based on the learning]
```
