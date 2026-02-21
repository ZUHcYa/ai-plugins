---
name: "review"
description: "Conduct disruption review for EXPLOIT phase canvases"
---

# /knvs:review

## Purpose

Reviews an EXPLOIT canvas for risks, opportunities, and disruption threats. Creates separate review files for history tracking.

## When to Use

- Quarterly reviews of running businesses
- When market conditions have changed
- Identifying disruption risks early

## Workflow

```
User: /knvs:review
Claude: Which business do you want to review?

        1. Invoice Tool (exploit/)
        2. AI Bookkeeping (exploit/)

User: 1
Claude: Starting Disruption Review...

        I'll analyze your business model across 5 areas:

        ## 1. Market & Competition
        - Current competitive landscape
        - New entrants / substitutes
        - Market share trends

        [Claude provides analysis and questions]

        ## 2. Technology
        ...

        ## 3. Regulation
        ...

        ## 4. Value Proposition
        ...

        ## 5. Revenue Streams
        ...

        ## External Factors
        I've identified these external factors:
        - EU AI Regulation 2027: Potential opportunity
        - Market consolidation: Medium risk

        Are there additional external factors to consider?

User: Yes, competitor just raised 5M

Claude: Noted. Now let's assess business model impact...

        ## Business Model Impact
        Based on findings, I recommend:
        - Value Proposition: Add DATEV integration
        - Revenue Streams: Improve premium tier

        Review saved: reviews/ai-bookkeeping/2026-01-26.md
        Canvas updated: exploit/ai-bookkeeping.md
        Next review: 2026-04-26
```

## Past Reviews

The skill can also list or display past reviews when the user asks
(e.g., "show past reviews for AI Bookkeeping" or "show the review from June 2026"):

```
User: /knvs:review  (then asks to see past reviews)
Claude: Reviews for AI Bookkeeping:

        | Date | Notes |
        |------|-------|
        | 2026-06-15 | Quarterly review |
        | 2026-03-15 | Initial review |

        Which review do you want to see?
```

## Impact Challenge Mode

After selecting a canvas for review, the skill offers impact-based analysis as an **interview dialog**.
The goal is awareness - not evaluating whether the canvas already covers impacts.
Canvas changes happen separately via `/knvs:ideate` (optionally as a forked canvas).

```
Claude: Would you like to challenge the canvas against Impact Atoms as well?

        Options:
        [A] ALL impacts - Full comparison against all atoms in impacts/
        [N] NEW impacts only - Only atoms created since the last review (2026-01-26)
        [S] Skip - Regular review without Impact Challenge

User: N
Claude: 2 impacts found. Let's go through them one by one.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 1 of 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Title: Semantic Vectors Instead of Images
        Severity in canvas: HIGH
        BMC fields: Value Proposition, Key Resources

        The scenario:
        Future buyers (AI agents) ignore pure HTML data.
        They require vectors and structured attributes.

        Pressure on this canvas:
        * Value Prop: Systems that only deliver "visuals" become invisible.
        * Key Resources: Vector databases become mandatory.

        How do you assess this impact currently?

User:   Relevant, but we've already started restructuring.

Claude: Got it. Does that mean Key Resources is already updated,
        or is that still pending?

User:   Still pending, but it's on the roadmap.

Claude: Understood. Moving to the next impact.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 2 of 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Title: Algorithmic Price Comparison
        Severity in canvas: – (not yet assessed, no callout)
        BMC fields: Revenue Streams

        The scenario:
        Price comparison becomes algorithmic instead of manual.

        Pressure on this canvas:
        * Revenue Streams: Commission models are bypassed by direct
          API negotiations.

        This impact is not yet embedded in the canvas.
        If relevant: embed via /knvs:ideate and assess severity
        in the BMC context.

        How do you assess this impact?

User:   Not relevant for our model.

Claude: Noted.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        2 impacts discussed.
        Continuing with the regular Disruption Review...
```

### How "New Since Last Review" Works

The skill determines "new" impacts by comparing:
- `created` date in each impact atom's frontmatter
- `review_date` of the most recent review file in `reviews/<canvas-name>/`
- If no previous review exists, ALL impacts are considered "new"

### Sources for the Impact Dialog

The interview dialog uses **two sources**:

**1. Canvas Inline Callouts (Severity)**
- Contains the context-specific severity for THIS canvas
- Severity was assigned during canvas creation (`/knvs:ideate`) in the BMC context
- Displayed in the dialog (when present)

**2. Impact Atoms in impacts/ (Completeness)**
- Lists ALL available impacts for this research
- Detects impacts not yet embedded in the canvas (no callout)
- Impact atoms contain NO severity (facts only)

**Dialog logic:**
1. Scan `impacts/` for all atoms that could affect this canvas (via `driver` + `bmc_fields`)
2. For EACH impact: check whether a canvas callout exists
3. Read severity from canvas callout (when present)
4. If no callout exists: mention in dialog, recommend embedding via `/knvs:ideate`
5. Discuss user assessment in dialog, no automatic evaluation

**Important:** The review does NOT modify the canvas. Adjustments are made separately via `/knvs:ideate`.

If unconfirmed market assumptions arise during the dialog
(e.g. "Competitor X will exit the market"), see
"Hypothese (Research-Vorlaeuferstufe)" in CLAUDE.md.

---

## What the Skill Does

1. Lists all canvases in `exploit/` folder
2. User selects which business to review
3. Claude analyzes and provides insights
4. Reviews 5 key areas:
   - Market & Competition
   - Technology trends
   - Regulation & Compliance
   - Value Proposition relevance
   - Revenue Streams stability
5. Identifies External Factors (market, tech, regulation, economy)
6. Assesses Business Model Impact
7. Generates risk matrix (probability x impact)
8. Creates action items
9. Saves review to `reviews/<canvas-name>/YYYY-MM-DD.md`
10. Updates canvas with review reference
11. Sets next review date (default: quarterly)

## Review Areas

| Area | Key Questions |
|------|---------------|
| Market | New competitors? Market shifts? |
| Technology | Disruptive tech? Build vs buy? |
| Regulation | New laws? Compliance gaps? |
| Value Prop | Still relevant? Customer needs changed? |
| Revenue | Pricing pressure? New models needed? |

## File Structure

Reviews are stored in separate files for history tracking:

```
<targetPath>/
|- exploit/
|   |- ai-bookkeeping.md         # Canvas (references reviews)
|- reviews/
    |- ai-bookkeeping/           # Folder per canvas
        |- 2026-03-15.md         # Past review
        |- 2026-06-15.md         # Latest review
```

## Backwards Compatibility

- Existing canvases with inline `## Review History` continue to work
- Skill can read both formats (inline + separate files)
- New reviews always go to `reviews/` folder

## Notes

- Review frequency is typically quarterly
- External Factors track macro-level influences

---

## Canvas Template

Canonical template for disruption reviews:

```markdown
---
canvas: [canvas-name]
review_date: YYYY-MM-DD
participants: []
next_review: YYYY-MM-DD
---

# Disruption Review: [Canvas Name]

**Canvas:** [Link to exploit/canvas.md]
**Date:** YYYY-MM-DD
**Participants:** [Names or "Solo analysis"]

---

## Findings

| Area | Status | Notes |
|------|--------|-------|
| Market & Competition | | |
| Technology | | |
| Regulation | | |
| Value Proposition | | |
| Revenue Streams | | |

---

## Risk Matrix

| Risk | Probability | Impact | Priority | Owner |
|------|-------------|--------|----------|-------|
| | Low/Med/High | Low/Med/High | | |

---

## Open Items

| Item | Raised By | Category | Status |
|------|-----------|----------|--------|
| | | Question/Concern/Idea | Open/Resolved |

---

## External Factors

| Factor | Type | Impact | Timeframe |
|--------|------|--------|-----------|
| | Market/Tech/Regulation/Economy | Positive/Negative/Uncertain | Near/Mid/Long-term |

---

## Business Model Impact

| BMC Area | Current State | Recommended Change | Priority |
|----------|---------------|-------------------|----------|
| | | | |

---

## Action Items

- [ ] @Owner: Task (Due: YYYY-MM-DD)

---

## Summary

[Brief narrative summary]

---

**Next Review:** YYYY-MM-DD
```

