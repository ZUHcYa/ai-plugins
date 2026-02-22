---
name: "assess"
description: "Run a Performance Assessment on an EXPLOIT canvas (10 dimensions, -3 to +3)"
---

# /knvs:assess

## Purpose

Runs a structured Performance Assessment on an EXPLOIT canvas based on
"The Invincible Company" methodology. Scores 10 dimensions from -3 to +3,
producing a total Performance Score (-30 to +30).

**Only applicable to EXPLOIT canvases.** EXPLORE canvases use the hypothesis loop instead.

## When to Use

- After moving a canvas to EXPLOIT phase via `/knvs:exploit`
- At regular checkpoints to track business model performance over time
- Before a strategic portfolio review
- **NOT for EXPLORE canvases** — these use `/knvs:hypothesize` and the validation loop

## Workflow

```
User: /knvs:assess
Claude: Which EXPLOIT canvas do you want to assess?

        1. AI Bookkeeping (exploit/) - last assessed: 2026-01-15
        2. Fitness App (exploit/) - never assessed

User: 1
Claude: Starting Performance Assessment for AI Bookkeeping.
        Rate each dimension from -3 to +3.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        DIMENSION 1 of 10: VALUE PROPOSITION
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        -3  Our products and services perform worse than
            those of our competition.

        +3  Our products and services are highly differentiated
            and loved by our customers.

        Score [-3 to +3]:

User: 2
Claude: Value Proposition: +2
        Optional note (or press Enter to skip):

User: Strong product-market fit, NPS 72

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        DIMENSION 2 of 10: CUSTOMER SEGMENTS
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        -3  We lost over 20% of our customer base
            in the last six months.

        +3  We increased our customer base by at least 50%
            over the last six months.

        Score [-3 to +3]:

User: 1

        [... continues for all 10 dimensions ...]

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        PERFORMANCE ASSESSMENT SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Canvas: exploit/ai-bookkeeping.md
        Date: 2026-02-22

        | # | Dimension              | Score |
        |---|------------------------|-------|
        | 1 | Value Proposition      |    +2 |
        | 2 | Customer Segments      |    +1 |
        | 3 | Channels               |    -1 |
        | 4 | Customer Relationships |     0 |
        | 5 | Key Resources          |    +2 |
        | 6 | Key Activities         |    +1 |
        | 7 | Key Partnerships       |    +1 |
        | 8 | Revenue Streams        |    +2 |
        | 9 | Cost Structure         |    -1 |
        |10 | Margins                |    +1 |
        |---|------------------------|-------|
        |   | PERFORMANCE SCORE      |    +8 |

        Previous assessment (2026-01-15): +5
        Trend: +3 improvement

        Saved: assessments/ai-bookkeeping/2026-02-22.md
        Updated: exploit/ai-bookkeeping.md (performance_score, last_assessment)
```

## What the Skill Does

1. Lists canvases in `exploit/` with last assessment date
2. User selects canvas
3. For each of the 10 dimensions:
   - Shows the -3 statement and the +3 statement
   - User chooses a score from -3 to +3 (integer)
   - User optionally adds a note for context
4. Calculates total Performance Score (sum of all 10 scores)
5. If a previous assessment exists, shows trend comparison
6. Creates assessment file in `assessments/<canvas-slug>/YYYY-MM-DD.md`
7. Updates canvas frontmatter: `performance_score` and `last_assessment`

## The 10 Dimensions

Each dimension maps to a BMC field. The scale runs from -3 (critical) to +3 (excellent).

| # | Dimension | -3 Statement | +3 Statement |
|---|-----------|-------------|-------------|
| 1 | **Value Proposition** | Our products and services perform worse than those of our competition. | Our products and services are highly differentiated and loved by our customers. |
| 2 | **Customer Segments** | We lost over 20% of our customer base in the last six months. | We increased our customer base by at least 50% over the last six months. |
| 3 | **Channels** | We are 100% dependent on intermediaries to get products and services to customers and they are making market access difficult. | We have direct market access and fully own the relationship with the customers of our products and services. |
| 4 | **Customer Relationships** | All our customers could theoretically leave us immediately, without incurring direct or indirect switching costs if they left. | All our customers are locked in for several years and they would incur significant direct and indirect switching costs if they left. |
| 5 | **Key Resources** | Our key resources are significantly inferior to those of our competitors and they have deteriorated over the last six months. New entrants compete with new, better, or cheaper resources. | Our key resources can't easily be copied or emulated for the next couple of years and they give us a competitive advantage (e.g., intellectual property, brand, etc.). |
| 6 | **Key Activities** | The performance of our key activities is significantly inferior to that of our competitors and has deteriorated over the last six months. New entrants compete with new, better, or cheaper activities. | Our key activities can't easily be copied or emulated for the next couple of years and they give us a competitive advantage (e.g., cost effectiveness, scale, etc.). |
| 7 | **Key Partnerships** | Over the last six months we lost access to key partners. | Our key partners are locked in for years to come. |
| 8 | **Revenue Streams** | We lost over 20% of our revenues in the last six months. | We doubled our revenues over the last six months and are growing significantly faster than our competitors. |
| 9 | **Cost Structure** | Our cost structure grew faster than revenues and is significantly less effective than that of our competitors. | Our cost structure shrunk compared to revenue growth and is significantly more effective than that of our competitors. |
| 10 | **Margins** | Our margins shrunk by over 50% in the last six months and/or are significantly lower than those of our competition (e.g., over 50% lower). | Our margins increased by at least 50% in the last six months and/or are significantly higher than those of our competition (e.g., over 50% higher). |

## Score Interpretation

| Score Range | Interpretation |
|-------------|---------------|
| +20 to +30 | Excellent — strong, defensible business model |
| +10 to +19 | Good — solid performance with room for improvement |
| 0 to +9 | Moderate — mixed signals, attention needed |
| -10 to -1 | Weak — significant challenges, action required |
| -30 to -11 | Critical — business model under severe pressure |

## Edge Cases

### No EXPLOIT canvases

```
Claude: No EXPLOIT canvases found.

        Move a validated canvas to EXPLOIT first with /knvs:exploit.
```

### Previous assessment exists (trend comparison)

When a previous assessment exists in `assessments/<canvas-slug>/`, the skill
reads the most recent one and shows a dimension-by-dimension trend comparison
in the summary.

```
        TREND (vs. 2026-01-15)
        -----------------------
        Value Proposition:      +1 → +2 (improved)
        Customer Segments:      +1 → +1 (stable)
        Channels:               -2 → -1 (improved)
        ...
        TOTAL:                  +5 → +8 (+3 improvement)
```

## Notes

- Assessments are individual .md files in `assessments/<canvas-slug>/`
- Filename format: `YYYY-MM-DD.md` (date-based, since assessments are repeated over time)
- Each assessment is a complete snapshot — all 10 dimensions scored
- The skill does NOT auto-score — the user makes all judgments
- Assessments can also be created manually in any Markdown editor
- Source: "The Invincible Company" by Osterwalder, Pigneur, Smith, Etiemble

---

## Assessment Template

Canonical template (also in CLAUDE.md):

```markdown
---
type: assessment
assessment_type: performance
canvas: exploit/canvas-slug.md
performance_score: 8
date: YYYY-MM-DD
---

# Performance Assessment: [Canvas Title]

## Date

YYYY-MM-DD

## Scores

| # | Dimension | Score | Note |
|---|-----------|-------|------|
| 1 | Value Proposition | +2 | Strong product-market fit, NPS 72 |
| 2 | Customer Segments | +1 | |
| 3 | Channels | -1 | Still dependent on partner network |
| 4 | Customer Relationships | 0 | |
| 5 | Key Resources | +2 | |
| 6 | Key Activities | +1 | |
| 7 | Key Partnerships | +1 | |
| 8 | Revenue Streams | +2 | |
| 9 | Cost Structure | -1 | |
| 10 | Margins | +1 | |

**Performance Score: +8**

## Trends

(compared to previous assessment, if available)

## Summary

[Brief narrative of overall business model health and key observations]
```
