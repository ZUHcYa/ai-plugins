---
name: "hypothesize"
description: "Extract and prioritize hypotheses from a BMC or verified research"
---

# /knvs:hypothesize

## Purpose

Extracts testable hypotheses from a Business Model Canvas and categorizes them
into Desirability, Feasibility, and Viability (Strategyzer methodology).
**Only applicable to EXPLORE canvases.** EXPLOIT canvases do not use hypotheses.

Two modes:
- **From BMC** — analyze canvas for implicit assumptions (default)
- **From Research** — test verified research findings against a canvas

## When to Use

- After setting an EXPLORE canvas to `status: testing`
- When you want to systematically identify assumptions in your BMC
- When adding new hypotheses to an existing EXPLORE canvas
- When a verified research report has findings relevant to an EXPLORE canvas
- **NOT for EXPLOIT canvases** — these have their own mechanisms (Performance & Trend Assessments)

## Workflow

```
User: /knvs:hypothesize
Claude: Which canvas do you want to analyze?

        1. AI Bookkeeping (explore/) - 0 hypotheses
        2. Fitness App (explore/) - 3 hypotheses

User: 1
Claude: How do you want to generate hypotheses?

        [B] From BMC (analyze canvas for implicit assumptions)
        [F] From Research (test verified research findings against this canvas)

User: B
Claude: Analyzing Business Model Canvas...

        I've identified 5 assumptions in your canvas:

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS 1 of 5
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Category: DESIRABILITY
        Claim: "Freelancers need automated receipt categorization"
        BMC Fields: Value Proposition, Customer Segments
        Importance: HIGH
        Confidence: —

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
        Confidence: —

        -> Accept? [Y/n/edit]

User: edit - importance should be MEDIUM
Claude: Updated:
        Importance: MEDIUM

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

        Priority (high importance + no confidence):
        1. receipt-categorization-need (Desirability, HIGH, —)
        2. ai-accuracy-requirement (Feasibility, HIGH, —)

        Next step: /knvs:experiment to design tests for priority hypotheses.
```

## Interaction Rules

**CRITICAL: One hypothesis at a time.**

1. Analyze the entire BMC (or research report) internally and identify all hypotheses first.
2. Tell the user how many hypotheses were found: `I've identified N assumptions in your canvas:`
3. Present **exactly ONE hypothesis**, then STOP and wait for the user's response (`[Y/n/edit]`).
4. Do NOT output the next hypothesis until the user has responded to the current one.
5. After the user responds (Y → save and confirm, n → skip, edit → apply changes and re-show):
   proceed to the next hypothesis.
6. After the last hypothesis: show the summary.

This ensures the user consciously reviews each assumption rather than scanning a wall of text.

---

## What the Skill Does

### From BMC (default)

1. Lists canvases in `explore/`
2. User selects canvas and mode [B]
3. Reads BMC content and identifies implicit assumptions per dimension
4. Categorizes each assumption:
   - **Desirability** (Value Proposition, Customer Segments, Channels, Customer Relationships)
   - **Feasibility** (Key Resources, Key Activities, Key Partnerships)
   - **Viability** (Revenue Streams, Cost Structure)
5. Proposes importance level (high/medium/low) based on how critical the assumption is
6. Sets initial confidence level (empty — not assessed, no experiments yet)
7. Presents each hypothesis ONE AT A TIME — waits for user confirmation (`Y/n/edit`) before
   proceeding to the next. Never outputs multiple hypotheses in one response.
8. Creates hypothesis files in `hypotheses/<canvas-slug>/`
9. Shows priority summary at the end

### From Research

1. Lists canvases in `explore/`
2. User selects canvas and mode [F]
3. Scans `research/` folder for files with `status: verified`
4. User selects a verified research report
5. Reads the `## Implications for Business Model` section
6. Extracts testable claims and maps them to BMC fields on the selected canvas
7. Sets initial confidence level to empty (confidence is based on own experiments, not external research)
8. Adds `source_research` to frontmatter
9. `## Evidence` section references the research report as context
10. Presents each hypothesis ONE AT A TIME — waits for user confirmation (`Y/n/edit`) before
    proceeding to the next. Never outputs multiple hypotheses in one response.
11. Creates hypothesis files in `hypotheses/<canvas-slug>/`
12. Shows priority summary at the end

## From Research Workflow

```
User: /knvs:hypothesize
Claude: Which canvas do you want to analyze?

        1. AI Bookkeeping (explore/) - 3 hypotheses
        2. Fitness App (explore/) - 0 hypotheses

User: 1
Claude: explore/ai-bookkeeping.md already has 3 hypotheses.

        Options:
        [A] Add more hypotheses (analyze BMC for uncovered assumptions)
        [F] From Research (test verified research findings against this canvas)
        [V] View existing hypotheses
        [R] Re-analyze entire BMC (existing hypotheses preserved)

User: F
Claude: Verified research reports:

        1. machine-customers (verified 2026-02-20)
        2. ai-bookkeeping-market (verified 2026-02-18)

User: 1
Claude: Reading "Implications for Business Model" from machine-customers...

        I've identified 2 testable claims against your canvas:

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        HYPOTHESIS 1 of 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Category: DESIRABILITY
        Claim: "Machine customers will autonomously purchase
               bookkeeping services by 2028"
        BMC Fields: Customer Segments, Channels
        Importance: HIGH
        Confidence: —
        Source: research/machine-customers.md

        -> Accept? [Y/n/edit]

User: Y
Claude: Saved: hypotheses/ai-bookkeeping/machine-customers-purchasing.md

        [... continues for remaining hypotheses ...]

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Canvas: explore/ai-bookkeeping.md
        Source: research/machine-customers.md
        Hypotheses created: 2 (from research)
          Desirability: 1
          Feasibility: 1

        All from-research hypotheses start with confidence: — (not assessed).
        Source research is tracked in frontmatter (source_research).

        Next step: /knvs:experiment to validate with your own data.
```

---

## Category-to-BMC-Field Mapping

| Category | BMC Fields | Core Question |
|----------|-----------|---------------|
| **Desirability** | Value Proposition, Customer Segments, Channels, Customer Relationships | Do customers want this? |
| **Feasibility** | Key Resources, Key Activities, Key Partnerships | Can we build/deliver this? |
| **Viability** | Revenue Streams, Cost Structure | Is this financially sustainable? |

## Prioritization

After all hypotheses are created, the skill shows a priority view:

```
Priority (importance × confidence):

HIGH IMPORTANCE + NO CONFIDENCE (test first):
  1. receipt-categorization-need (Desirability)
  2. ai-accuracy-requirement (Feasibility)

HIGH IMPORTANCE + LOW CONFIDENCE (validate further):
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
        [F] From Research (test verified research findings against this canvas)
        [V] View existing hypotheses
        [R] Re-analyze entire BMC (existing hypotheses preserved)
```

### No verified research reports (From Research mode)

```
Claude: No verified research reports found in research/.

        Only reports with status: verified are accepted.
        Use /research:finalize to verify draft reports first.
```

### Canvas has no content

```
Claude: explore/ai-bookkeeping.md has empty BMC sections.
        Cannot extract hypotheses from an empty canvas.

        Fill in your BMC sections first, then run /knvs:hypothesize again.
```

## Notes

- Hypotheses are individual .md files in `hypotheses/<canvas-slug>/`
- Each hypothesis has frontmatter with confidence, importance, category, status
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
confidence:
importance: high
status: open
source_research: research/report-slug.md  # optional, only for From Research mode
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
(From Research: context from research/report-slug.md § Implications for Business Model)

## Related Experiments

(none yet)
```
