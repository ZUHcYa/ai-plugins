---
name: "assess"
description: "Run a Performance or Trend Assessment on an EXPLOIT canvas (10 dimensions each, -3 to +3)"
---

# /knvs:assess

## Purpose

Runs structured assessments on EXPLOIT canvases based on "The Invincible Company"
methodology. Two assessment types provide complementary perspectives:

- **Performance** — Current snapshot (Ist-Aufnahme). 10 BMC-mapped dimensions.
- **Trend** — Future projection (Zukunftsprojektion). 10 external trend dimensions.

Each assessment scores 10 dimensions from -3 to +3, producing a total score (-30 to +30).

**Only applicable to EXPLOIT canvases.** EXPLORE canvases use the hypothesis loop instead.

## When to Use

- After moving a canvas to EXPLOIT phase via `/knvs:exploit`
- At regular checkpoints to track business model health and risk over time
- Before a strategic portfolio review
- **NOT for EXPLORE canvases** — these use `/knvs:hypothesize` and the validation loop

## Workflow

```
User: /knvs:assess
Claude: Which EXPLOIT canvas do you want to assess?

        1. AI Bookkeeping (exploit/)
           Performance: +8 (2026-01-15) | Trend: never assessed
        2. Fitness App (exploit/)
           Performance: never assessed | Trend: never assessed

User: 1
Claude: What type of assessment?

        [P] Performance (current business model health)
        [T] Trend (future threats and opportunities)

User: T
Claude: Starting Trend Assessment for AI Bookkeeping.
        Rate each dimension from -3 to +3.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        DIMENSION 1 of 10: SUBSTITUTION RISK
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        -3  New entrants are gaining traction with cheaper, better,
            or substitute products and services that may make our
            business model obsolete.

        +3  Competition for our products and services is shrinking
            and our products and services are likely to gain
            traction and benefit from that.

        Score [-3 to +3]:

User: -2
Claude: Substitution Risk: -2
        Optional note (or press Enter to skip):

User: Two AI-native competitors launched in Q4

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        DIMENSION 2 of 10: MARKET TRAJECTORY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        -3  The markets in which we are active are projected
            to shrink significantly over the coming years.

        +3  The markets in which we are active are projected
            to grow significantly over the coming years.

        Score [-3 to +3]:

User: 2

        [... continues for all 10 dimensions ...]

Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        TREND ASSESSMENT SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Canvas: exploit/ai-bookkeeping.md
        Date: 2026-02-22

        | # | Dimension              | Score |
        |---|------------------------|-------|
        | 1 | Substitution Risk      |    -2 |
        | 2 | Market Trajectory      |    +2 |
        | 3 | Customer Friction      |    +1 |
        | 4 | Social/Cultural Trends |     0 |
        | 5 | Technology Trends      |    -1 |
        | 6 | Regulatory Environment |    +1 |
        | 7 | Supply Chain           |     0 |
        | 8 | Economic Resilience    |    +1 |
        | 9 | Geopolitical Dep.      |    +2 |
        |10 | VC/Startup Activity    |    -2 |
        |---|------------------------|-------|
        |   | TREND SCORE            |    +2 |

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        ASSESSMENT OVERVIEW
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Performance (current snapshot): +8 (2026-01-15)
        Trend (future projection):      +2 (2026-02-22)

        Saved: assessments/ai-bookkeeping/2026-02-22-trend.md
        Updated: exploit/ai-bookkeeping.md (trend_score, last_assessment)
```

## What the Skill Does

1. Lists canvases in `exploit/` with last assessment dates (both types)
2. User selects canvas
3. User selects assessment type: **[P] Performance** or **[T] Trend**
4. For each of the 10 dimensions (type-specific):
   - Shows the -3 statement and the +3 statement
   - User chooses a score from -3 to +3 (integer)
   - User optionally adds a note for context
5. Calculates total score (sum of all 10 scores)
6. If a previous assessment of the same type exists, shows trend comparison
7. If both assessment types exist, shows combined overview
8. Creates assessment file in `assessments/<canvas-slug>/YYYY-MM-DD-<type>.md`
9. Updates canvas frontmatter: `performance_score` or `trend_score` + `last_assessment`

---

## Performance Assessment: The 10 Dimensions

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

### Performance Score Interpretation

| Score Range | Interpretation |
|-------------|---------------|
| +20 to +30 | Excellent — strong, defensible business model |
| +10 to +19 | Good — solid performance with room for improvement |
| 0 to +9 | Moderate — mixed signals, attention needed |
| -10 to -1 | Weak — significant challenges, action required |
| -30 to -11 | Critical — business model under severe pressure |

---

## Trend Assessment: The 10 Dimensions

Each dimension assesses an external threat or opportunity. The scale runs from -3 (high disruption risk) to +3 (low risk / favorable trends).

| # | Dimension | -3 Statement | +3 Statement |
|---|-----------|-------------|-------------|
| 1 | **Substitution Risk** | New entrants are gaining traction with cheaper, better, or substitute products and services that may make our business model obsolete. | Competition for our products and services is shrinking and our products and services are likely to gain traction and benefit from that. |
| 2 | **Market Trajectory** | The markets in which we are active are projected to shrink significantly over the coming years. | The markets in which we are active are projected to grow significantly over the coming years. |
| 3 | **Customer Friction** | Various trends (tech, cultural, demographics) are reducing the friction for our customers to leave us and never come back. | Various trends are making it harder for our customers to desert us and the friction for them to leave is increasing. |
| 4 | **Social/Cultural Trends** | Social and cultural trends that are projected to grow are driving customers away from us (e.g., sustainability, fashion, etc.). | Growing social and cultural trends are projected to attract customers toward our offerings and strengthen demand for our value proposition. |
| 5 | **Technology Trends** | Technology trends that substantially undermine our business model or make it obsolete are gaining traction. | Technology trends that substantially strengthen our business model are gaining traction. |
| 6 | **Regulatory Environment** | New regulations make our business model significantly more expensive or impossible to operate and give our competitors an advantage. | New regulations make our business model significantly cheaper or easier to operate and give us a competitive advantage over our competitors. |
| 7 | **Supply Chain** | Suppliers and value chain actors are changing in a way that puts our business model at risk. | Suppliers and value chain actors are changing in a way that radically strengthens our business model. |
| 8 | **Economic Resilience** | An economic downturn in the next six months would be lethal to our business model (e.g., due to high cost structure, debt obligations, etc.). | Our business model is resilient and would even benefit if an economic downturn happened in the next six months (e.g., due to weak competitors). |
| 9 | **Geopolitical Dependencies** | Our business model depends on key resources or other factors that may be affected by geopolitical or other external forces (e.g., commodity prices, trade wars, etc.). | Our business model does not depend on key resources or other factors that are affected by geopolitical or other external forces (e.g., commodity prices, trade wars, etc.). |
| 10 | **VC/Startup Activity** | There is a significant amount of venture capital funding start-ups in our arena and this has grown over the last six months. | There is little to no venture capital funding start-ups in our arena. |

### Trend Score Interpretation

| Score Range | Interpretation |
|-------------|---------------|
| +20 to +30 | Very safe — minimal disruption risk, favorable trends |
| +10 to +19 | Safe — low risk with some areas to monitor |
| 0 to +9 | Moderate — mixed signals, some threats emerging |
| -10 to -1 | At risk — significant threats, proactive action needed |
| -30 to -11 | High risk — severe disruption threats, urgent action required |

---

## Combined View

When both assessments exist for a canvas, the skill shows the combined position:

```
ASSESSMENT OVERVIEW
━━━━━━━━━━━━━━━━━━
Performance Score (current snapshot):    +8
Trend Score (future projection):         +2
```

Together they answer: **How well is the business doing today, and what does the future look like?**

---

## Edge Cases

### No EXPLOIT canvases

```
Claude: No EXPLOIT canvases found.

        Move a validated canvas to EXPLOIT first with /knvs:exploit.
```

### Previous assessment exists (trend comparison)

When a previous assessment of the same type exists in `assessments/<canvas-slug>/`,
the skill reads the most recent one and shows a dimension-by-dimension comparison.

```
        TREND COMPARISON (vs. 2026-01-15)
        -----------------------------------
        Substitution Risk:      -1 → -2 (worsened)
        Market Trajectory:      +2 → +2 (stable)
        Customer Friction:      +1 → +1 (stable)
        ...
        TOTAL:                  +5 → +2 (-3 decline)
```

## Notes

- Assessments are individual .md files in `assessments/<canvas-slug>/`
- Filename format: `YYYY-MM-DD-performance.md` or `YYYY-MM-DD-trend.md`
- Each assessment is a complete snapshot — all 10 dimensions scored
- The skill does NOT auto-score — the user makes all judgments
- Assessments can also be created manually in any Markdown editor
- Source: "The Invincible Company" by Osterwalder, Pigneur, Smith, Etiemble

---

## Performance Assessment Template

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

---

## Trend Assessment Template

```markdown
---
type: assessment
assessment_type: trend
canvas: exploit/canvas-slug.md
trend_score: 2
date: YYYY-MM-DD
---

# Trend Assessment: [Canvas Title]

## Date

YYYY-MM-DD

## Scores

| # | Dimension | Score | Note |
|---|-----------|-------|------|
| 1 | Substitution Risk | -2 | Two AI-native competitors launched in Q4 |
| 2 | Market Trajectory | +2 | |
| 3 | Customer Friction | +1 | |
| 4 | Social/Cultural Trends | 0 | |
| 5 | Technology Trends | -1 | AI commoditization risk |
| 6 | Regulatory Environment | +1 | |
| 7 | Supply Chain | 0 | |
| 8 | Economic Resilience | +1 | |
| 9 | Geopolitical Dependencies | +2 | |
| 10 | VC/Startup Activity | -2 | Significant VC in fintech/AI space |

**Trend Score: +2**

## Trends

(compared to previous assessment, if available)

## Summary

[Brief narrative of external threats, opportunities, and disruption risk]
```
