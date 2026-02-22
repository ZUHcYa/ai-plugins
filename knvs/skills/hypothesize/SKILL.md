---
name: "hypothesize"
description: "Extract and prioritize hypotheses from a Business Model Canvas"
---

# /knvs:hypothesize

## Purpose

Extracts testable hypotheses from a Business Model Canvas and categorizes them
into Desirability, Feasibility, and Viability (Strategyzer methodology).

## When to Use

- After setting a canvas to `status: testing`
- When you want to systematically identify assumptions in your BMC
- When adding new hypotheses to an existing canvas

## Workflow

```
User: /knvs:hypothesize
Claude: Which canvas do you want to analyze?

        1. AI Bookkeeping (explore/) - 0 hypotheses
        2. Fitness App (explore/) - 3 hypotheses

User: 1
Claude: Analyzing Business Model Canvas...

        I've identified 5 assumptions in your canvas:

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS 1 of 5
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Category: DESIRABILITY
        Claim: "Freelancers need automated receipt categorization"
        BMC Fields: Value Proposition, Customer Segments
        Importance: HIGH
        Evidence: NONE

        -> Accept? [Y/n/edit]

User: Y
Claude: Saved: hypotheses/ai-bookkeeping/receipt-categorization-need.md

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS 2 of 5
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Category: VIABILITY
        Claim: "Customers will pay EUR 15-30/month"
        BMC Fields: Revenue Streams
        Importance: HIGH
        Evidence: NONE

        -> Accept? [Y/n/edit]

User: edit - importance should be MEDIUM, we have some market data
Claude: Updated:
        Importance: MEDIUM
        Evidence: WEAK

        -> Accept? [Y/n/edit]

User: Y
Claude: Saved: hypotheses/ai-bookkeeping/customers-will-pay-monthly.md

        [... continues for remaining hypotheses ...]

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Canvas: explore/ai-bookkeeping.md
        Hypotheses created: 5
          Desirability: 2
          Feasibility: 1
          Viability: 2

        Priority (high importance + no evidence):
        1. receipt-categorization-need (Desirability, HIGH, NONE)
        2. ai-accuracy-requirement (Feasibility, HIGH, NONE)

        Next step: /knvs:experiment to design tests for priority hypotheses.
```

## What the Skill Does

1. Lists canvases in `explore/`
2. User selects canvas
3. Reads BMC content and identifies implicit assumptions per dimension
4. Categorizes each assumption:
   - **Desirability** (Value Proposition, Customer Segments, Channels, Customer Relationships)
   - **Feasibility** (Key Resources, Key Activities, Key Partnerships)
   - **Viability** (Revenue Streams, Cost Structure)
5. Proposes importance level (high/medium/low) based on how critical the assumption is
6. Sets initial evidence level (default: none)
7. User confirms, rejects, or edits each hypothesis
8. Creates hypothesis files in `hypotheses/<canvas-slug>/`
9. Shows priority summary at the end

## Category-to-BMC-Field Mapping

| Category | BMC Fields | Core Question |
|----------|-----------|---------------|
| **Desirability** | Value Proposition, Customer Segments, Channels, Customer Relationships | Do customers want this? |
| **Feasibility** | Key Resources, Key Activities, Key Partnerships | Can we build/deliver this? |
| **Viability** | Revenue Streams, Cost Structure | Is this financially sustainable? |

## Prioritization

After all hypotheses are created, the skill shows a priority view:

```
Priority (importance × evidence):

HIGH IMPORTANCE + NO EVIDENCE (test first):
  1. receipt-categorization-need (Desirability)
  2. ai-accuracy-requirement (Feasibility)

HIGH IMPORTANCE + WEAK EVIDENCE (validate further):
  3. customers-will-pay-monthly (Viability)

MEDIUM/LOW IMPORTANCE (test later):
  4. linkedin-channel-effectiveness (Desirability)
  5. cloud-hosting-costs (Viability)
```

## Edge Cases

### Canvas already has hypotheses

```
Claude: explore/ai-bookkeeping.md already has 3 hypotheses.

        Options:
        [A] Add more hypotheses (analyze BMC for uncovered assumptions)
        [V] View existing hypotheses
        [R] Re-analyze entire BMC (existing hypotheses preserved)
```

### Canvas has no content

```
Claude: explore/ai-bookkeeping.md has empty BMC sections.
        Cannot extract hypotheses from an empty canvas.

        Fill in your BMC sections first, then run /knvs:hypothesize again.
```

## Notes

- Hypotheses are individual .md files in `hypotheses/<canvas-slug>/`
- Each hypothesis has frontmatter with evidence, importance, category, status
- The skill does NOT run experiments — use `/knvs:experiment` for that
- Hypotheses can also be created manually in any Markdown editor

---

## Hypothesis Template

Canonical template (also in CLAUDE.md):

```markdown
---
type: hypothesis
title: "[Testable statement]"
canvas: explore/canvas-slug.md
category: desirability|feasibility|viability
bmc_fields:
  - Value Proposition
  - Customer Segments
evidence: none
importance: high
status: open
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Hypothesis: [Title]

## Claim

[Clear, testable statement with context]

## Context

[Why this matters, which BMC dimensions are affected,
what happens if this hypothesis is invalidated]

## Evidence

No evidence yet. Requires experiment.

## Related Experiments

(none yet)
```
