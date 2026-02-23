---
name: "card"
description: "Create Learning Cards — per experiment or per hypothesis (with Persevere/Pivot/Kill decision)"
---

# /knvs:card

## Purpose

Creates Learning Cards in the classic "Testing Business Ideas" 4-part narrative.
Two modes:

- **Experiment Card** — Synthesizes one experiment into a learning card
- **Hypothesis Card** — Concludes the testing cycle of one hypothesis with a strategic decision (Persevere/Pivot/Kill)

**Only applicable to EXPLORE canvases.**

## When to Use

- After `/knvs:learn` — when insights exist for a completed experiment
- To conclude a hypothesis testing cycle with a strategic decision
- **NOT for EXPLOIT canvases**

## Mode Selection

```
User: /knvs:card
Claude: [E] Experiment Card — Learning Card for a single experiment
        [H] Hypothesis Card — Conclude hypothesis testing cycle (incl. decision)
```

---

## [E] Experiment Card

### Workflow

```
User: /knvs:card → E
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
        - Pricing Survey with 50 Freelancers → /knvs:card [E]

        Next steps:
        - /knvs:card [E] for remaining experiments
        - /knvs:card [H] to conclude a hypothesis testing cycle
```

### What the Experiment Card Does

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

### Multiple Insights per Experiment

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

---

## [H] Hypothesis Card

### Workflow

```
User: /knvs:card → H
Claude: Which canvas has hypotheses ready to conclude?

        1. AI Bookkeeping (explore/) - 3 validated, 1 invalidated, 1 open
        2. Fitness App (explore/) - 1 validated, 0 invalidated, 2 open

User: 1
Claude: ======================================
        HYPOTHESIS DASHBOARD: AI Bookkeeping
        ======================================

        HYPOTHESES
        ----------
        ✅ receipt-categorization-need (Desirability, HIGH, VALIDATED)
           -> Hypothesis learning card exists
        ✅ ai-accuracy-requirement (Feasibility, HIGH, VALIDATED)
           -> No hypothesis learning card yet
        ✅ customers-will-pay-monthly (Viability, MODERATE, VALIDATED)
           -> No hypothesis learning card yet
        ❌ linkedin-channel-effectiveness (Desirability, LOW, INVALIDATED)
           -> No hypothesis learning card yet
        ⬜ cloud-hosting-costs (Viability, —, OPEN)

        CONFIDENCE SUMMARY
        ------------------
        Validated: 3 (Desirability: 1, Feasibility: 1, Viability: 1)
        Invalidated: 1 (Desirability: 1)
        Open: 1 (Viability: 1)

        Select hypothesis to conclude (validated or invalidated only):

User: 2 (ai-accuracy-requirement)
Claude: Reading hypothesis, all experiments, and insights...

        Hypothesis: AI accuracy meets 90% categorization threshold
        Experiments: 2 (Prototype Test, Beta User Study)
        Insights: 3
        Status: VALIDATED
        Confidence: HIGH

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS LEARNING CARD DRAFT
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        ## We believed that...

        Our AI model can achieve 90%+ accuracy in automatic receipt
        categorization for German freelancer bookkeeping categories.

        -> Accept? [Y/n/edit]

User: Y
Claude: ## We observed...

        Prototype test with 500 receipts: 87% accuracy (below target).
        After model retraining with expanded dataset, beta user study
        with 20 users over 4 weeks: 94% accuracy across all categories.
        Highest accuracy in "Travel" (98%), lowest in "Office Supplies" (89%).

        -> Accept? [Y/n/edit]

User: Y
Claude: ## From that we learned...

        AI-based receipt categorization is feasible at production quality.
        Initial model needed retraining with domain-specific data, but
        achieved 94% accuracy — exceeding the 90% threshold. Category-
        specific accuracy varies, with "Office Supplies" requiring the
        most training data.

        -> Accept? [Y/n/edit]

User: Y
Claude: ## Therefore we will...

        What is your strategic decision for the business model?

        [P] PERSEVERE — Continue testing remaining hypotheses
        [V] PIVOT — Change direction based on this learning
        [K] KILL — Abandon this business model

User: P
Claude: Why persevere? What happens next?

User: Core feasibility confirmed. Next priority: validate cloud
      hosting costs to complete the viability picture.

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS LEARNING CARD SAVED
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        File: learning-cards/ai-bookkeeping/ai-accuracy-requirement.md
        Decision: PERSEVERE

        Decision logged in canvas:

        ### 2026-02-23 — Persevere

        **Kontext:** AI accuracy hypothesis validated (94% > 90% target).
        3/5 hypotheses validated, 1 invalidated, 1 open.
        **Entscheidung:** Core feasibility confirmed. Continue testing.
        **Naechste Schritte:**
        - Design experiment for cloud-hosting-costs hypothesis
        - Complete viability validation

        Updated: explore/ai-bookkeeping.md

        Remaining hypotheses without learning cards:
        - customers-will-pay-monthly (VALIDATED) → /knvs:card [H]
        - linkedin-channel-effectiveness (INVALIDATED) → /knvs:card [H]
```

### Pivot Flow

```
User: V (PIVOT)
Claude: What aspect of the business model needs to change?

User: The channel strategy needs a complete rethink. Instead of
      direct-to-consumer, we should go B2B through tax advisors.

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS LEARNING CARD SAVED
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        File: learning-cards/ai-bookkeeping/linkedin-channel-effectiveness.md
        Decision: PIVOT

        Creating pivot canvas...

        Original: explore/ai-bookkeeping.md → archived
        New canvas: explore/ai-bookkeeping-b2b.md
        Pivot from: explore/ai-bookkeeping.md

        Decision logged in both canvases:

        ### 2026-02-23 — Pivot

        **Kontext:** LinkedIn channel hypothesis invalidated.
        Core value prop validated but distribution needs rethinking.
        **Entscheidung:** Pivot from B2C to B2B distribution via
        tax advisors. Value proposition stays, channel changes.
        **Naechste Schritte:** Fill B2B canvas, rethink partnerships

        Next steps:
        - Edit explore/ai-bookkeeping-b2b.md with B2B model
        - Set status: testing when done
        - /knvs:hypothesize to extract hypotheses
```

### Kill Flow

```
User: K (KILL)
Claude: Are you sure? This will archive the canvas and all related data.

User: Yes, the market is too small.

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS LEARNING CARD SAVED
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        File: learning-cards/ai-bookkeeping/customers-will-pay-monthly.md
        Decision: KILL

        Canvas archived: explore/ai-bookkeeping.md → archive/ai-bookkeeping.md

        Decision logged:

        ### 2026-02-23 — Kill

        **Kontext:** Market validation showed insufficient demand.
        **Entscheidung:** Market too small for viable business model.
        **Naechste Schritte:** None — archived.

        Hypotheses, experiments, and insights remain in their folders
        for reference.
```

### What the Hypothesis Card Does

1. Lists canvases in `explore/` with validated or invalidated hypotheses
2. User selects canvas
3. Shows hypothesis dashboard:
   - Hypothesis status overview with confidence level (validated/invalidated/open)
   - Confidence summary by category (D/F/V)
   - Which hypotheses already have learning cards
4. User selects hypothesis (validated or invalidated only)
5. Reads ALL experiments and insights linked to that hypothesis
6. Generates draft for first 3 sections from aggregated source data
7. User confirms, rejects, or edits each section
8. "Therefore we will..." → User selects Persevere/Pivot/Kill + provides reasoning
9. Creates learning card file in `learning-cards/<canvas-slug>/`
10. Adds decision entry to `## Decisions` section in canvas
11. Executes decision consequences:
    - **Persevere:** Suggests next steps (remaining hypotheses, experiments)
    - **Pivot:** Creates new canvas in `explore/` with `status: draft` and `pivot_from` reference. Original moves to `archive/`. Decision logged in both.
    - **Kill:** Moves canvas to `archive/`. Hypotheses, experiments, insights remain for reference.

### Decision Criteria Guidance

The skill does NOT make the decision — it presents confidence levels and evidence, and the user decides.
However, it can suggest based on patterns:

| Pattern | Suggestion |
|---------|------------|
| All D/F/V validated | Consider `/knvs:exploit` |
| Core D validated, F/V open | Persevere — test remaining |
| Core D invalidated | Consider Pivot or Kill |
| V invalidated | Consider Pivot (pricing/revenue model change) |
| All hypotheses invalidated | Consider Kill |

---

## Edge Cases

### Experiment has no insights yet (Experiment Card)

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

### No completed experiments with insights (Experiment Card)

```
Claude: No completed experiments with insights found for any EXPLORE canvas.

        Complete experiments and extract insights first:
        - /knvs:capture to document experiment results
        - /knvs:learn to extract insights
```

### No validated/invalidated hypotheses (Hypothesis Card)

```
Claude: No validated or invalidated hypotheses found for any EXPLORE canvas.

        Complete the testing cycle first:
        - /knvs:experiment to design tests
        - /knvs:capture to run experiments
        - /knvs:learn to extract insights and update hypothesis status
```

### Hypothesis has no experiments (Hypothesis Card)

```
Claude: Hypothesis "cloud-hosting-costs" has no completed experiments.

        Run experiments first:
        - /knvs:experiment to design a test
        - /knvs:capture to start and complete it
        - /knvs:learn to extract insights
```

---

## Notes

### Experiment Cards
- One learning card per experiment (1:1 relationship)
- Filename matches the experiment slug
- The first 3 sections are pre-filled from source artifacts — the user confirms or edits
- "Therefore we will..." is always user-provided (never auto-generated)

### Hypothesis Cards
- One learning card per hypothesis (1:1 relationship)
- Filename matches the hypothesis slug
- Aggregates ALL experiments and insights for the hypothesis
- "Therefore we will..." is a structured decision (Persevere/Pivot/Kill) + user reasoning
- The decision triggers actions (archive for Kill, new canvas for Pivot)
- Decisions are logged in the canvas `## Decisions` section

### Both Types
- Learning cards are standalone and can be created manually in any Markdown editor
- The skill reads existing artifacts — it does NOT generate fake data

---

## Experiment Card Template

```markdown
---
type: learning-card
scope: experiment
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

## Hypothesis Card Template

```markdown
---
type: learning-card
scope: hypothesis
title: "[Hypothesis title]"
canvas: explore/canvas-slug.md
hypothesis: hypotheses/canvas-slug/hypothesis-slug.md
experiments:
  - experiments/canvas-slug/exp-1.md
  - experiments/canvas-slug/exp-2.md
insights:
  - insights/canvas-slug/insight-1.md
  - insights/canvas-slug/insight-2.md
decision: persevere | pivot | kill
confidence: very-low | low | moderate | high
created: YYYY-MM-DD
---

# Learning Card: [Hypothesis Title]

## We believed that...

[From hypothesis claim — the testable assumption]

## We observed...

[Aggregated evidence from all experiments — what actually happened]

## From that we learned...

[Synthesized from all insight(s) — key learnings]

## Therefore we will...

**[Persevere|Pivot|Kill]**

[User reasoning for the decision and what happens next]
```
