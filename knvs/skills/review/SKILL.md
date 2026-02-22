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

        1. AI Bookkeeping (exploit/)
        2. Invoice Tool (exploit/)

User: 1
Claude: Starting Disruption Review...

        I'll analyze your business model across 5 areas:

        ## 1. Market & Competition
        [Claude provides analysis and questions]

        ## 2. Technology
        ...

        ## 3. Regulation
        ...

        ## 4. Value Proposition
        ...

        ## 5. Revenue Streams
        ...

        Review saved: reviews/ai-bookkeeping/2026-02-22.md
        Canvas updated: exploit/ai-bookkeeping.md
        Next review: 2026-05-22
```

## Past Reviews

The skill can also list or display past reviews when the user asks:

```
User: /knvs:review  (then asks to see past reviews)
Claude: Reviews for AI Bookkeeping:

        | Date | Notes |
        |------|-------|
        | 2026-06-15 | Quarterly review |
        | 2026-03-15 | Initial review |

        Which review do you want to see?
```

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

## Notes

- Review frequency is typically quarterly
- External Factors track macro-level influences

---

## Review Template

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
