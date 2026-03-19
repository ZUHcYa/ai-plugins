---
name: "hypothesize"
description: "Extract and prioritize hypotheses from a BMC or verified research"
---

# /knvs:hypothesize

## Purpose

Extracts testable hypotheses from a Canvas and categorizes them.
Default categories for BMC canvases: Desirability, Feasibility, Viability.
For other canvas types, categories are derived from the canvas `##` headings.
**Only applicable to EXPLORE canvases.** EXPLOIT canvases do not use hypotheses.

Two modes:
- **From BMC** — analyze canvas for implicit assumptions (default)
- **From Research** — test verified research findings against a canvas

## When to Use

- After setting an EXPLORE canvas to `status: testing`
- When you want to systematically identify assumptions in your canvas
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
Claude: Saved: explore/ai-bookkeeping/hypotheses/receipt-categorization-need.md

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
Claude: Saved: explore/ai-bookkeeping/hypotheses/customers-will-pay-monthly.md

        [... continues for remaining hypotheses ...]

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Canvas: ai-bookkeeping (explore/)
        Hypotheses created: 5
          Desirability: 2
          Feasibility: 1
          Viability: 2

        Priority (high importance + no confidence):
        1. receipt-categorization-need (Desirability, HIGH, —)
        2. ai-accuracy-requirement (Feasibility, HIGH, —)

        Domain coverage:
          Product ✓  Design —  Tech ✓  Data —
          Legal —  Sales —  Marketing —  Finance ✓

        Blind spots: Design, Data, Legal, Sales, Marketing
        have no hypotheses. Consider if these are relevant for this canvas.

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
3. Reads canvas content and identifies implicit assumptions per section.
   Applies **Domain Lenses** (see below) to each canvas section to surface
   assumptions that pure canvas analysis might miss (Legal, Finance, Data, Design, etc.)
4. Categorizes each assumption based on canvas headings:
   - For BMC canvases: **Desirability** (Value Proposition, Customer Segments, Channels, Customer Relationships), **Feasibility** (Key Resources, Key Activities, Key Partnerships), **Viability** (Revenue Streams, Cost Structure)
   - For other canvas types: categories derived from `##` headings
5. Proposes importance level (high/medium/low) based on how critical the assumption is
6. Sets initial confidence level (empty — not assessed, no experiments yet)
7. Presents each hypothesis ONE AT A TIME — waits for user confirmation (`Y/n/edit`) before
   proceeding to the next. Never outputs multiple hypotheses in one response.
8. Creates hypothesis files in `explore/<canvas-slug>/hypotheses/` (creates subfolder if needed)
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
11. Creates hypothesis files in `explore/<canvas-slug>/hypotheses/` (creates subfolder if needed)
12. Shows priority summary at the end

## From Research Workflow

```
User: /knvs:hypothesize
Claude: Which canvas do you want to analyze?

        1. AI Bookkeeping (explore/) - 3 hypotheses
        2. Fitness App (explore/) - 0 hypotheses

User: 1
Claude: explore/ai-bookkeeping/ai-bookkeeping.md already has 3 hypotheses.

        Options:
        [A] Add more hypotheses (analyze canvas for uncovered assumptions)
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
Claude: Saved: explore/ai-bookkeeping/hypotheses/machine-customers-purchasing.md

        [... continues for remaining hypotheses ...]

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Canvas: ai-bookkeeping (explore/)
        Source: research/machine-customers.md
        Hypotheses created: 2 (from research)
          Desirability: 1
          Feasibility: 1

        All from-research hypotheses start with confidence: — (not assessed).
        Source research is tracked in frontmatter (source_research).

        Next step: /knvs:experiment to validate with your own data.
```

---

## Default Category Mapping (BMC Canvases)

For BMC canvases, the default category-to-field mapping is:

| Category | Canvas Fields | Core Question |
|----------|-----------|---------------|
| **Desirability** | Value Proposition, Customer Segments, Channels, Customer Relationships | Do customers want this? |
| **Feasibility** | Key Resources, Key Activities, Key Partnerships | Can we build/deliver this? |
| **Viability** | Revenue Streams, Cost Structure | Is this financially sustainable? |

For non-BMC canvases, suggest categories based on the canvas `##` headings.

## Domain Lenses

During hypothesis generation, apply cross-functional perspectives to each canvas section.
Each lens-question that uncovers a non-trivial assumption becomes a hypothesis.
Not every lens is relevant for every canvas — skip obviously irrelevant ones
(e.g. Legal for a pure content project without user data).

The following table shows default lenses for BMC canvases (mapped to D/F/V).
For non-BMC canvases, adapt the lens questions to the canvas's own section headings.

| Domain | Desirability | Feasibility | Viability |
|--------|-------------|-------------|-----------|
| **Product** | Is the problem real, frequent, urgent? | — | — |
| **Design** | Is the interaction intuitive for the segment? | — | — |
| **Tech** | — | Can we build this with current stack/team? Technical risk? | — |
| **Data** | — | Do we have the required data infrastructure? ML feasibility? | — |
| **Legal** | — | Regulatory, compliance, IP, data privacy blockers? | Regulatory costs, license fees? |
| **Sales** | Can we reach the segment through our channels? | — | Sales cycle length, CAC, conversion? |
| **Marketing** | Is the messaging compelling? Brand fit? | — | Channel economics, acquisition costs? |
| **Research** | What do market data say about demand? | — | — |
| **Finance** | — | — | Unit economics, margins, capital expenditure, break-even? |

**Domain coverage in summary:** After all hypotheses are created, derive which domains are
covered from `canvas_fields`, `category`, and claim content. Show coverage as checklist with
blind spot hints (informational only, never an error).

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
Claude: explore/ai-bookkeeping/ai-bookkeeping.md already has 3 hypotheses.

        Options:
        [A] Add more hypotheses (analyze canvas for uncovered assumptions)
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
Claude: explore/ai-bookkeeping.md has no content sections.
        Cannot extract hypotheses from an empty canvas.

        Fill in your canvas sections first, then run /knvs:hypothesize again.
```

## Notes

- Hypotheses are individual .md files in `<phase>/<canvas-slug>/hypotheses/`
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
canvas: canvas-slug
category: desirability
canvas_fields:
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
