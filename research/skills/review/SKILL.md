---
name: "review"
description: "Systematic review lite - search multiple sources on a research question, compare findings, document contradictions"
---

# /research:review

## Purpose

Conducts a systematic-review-lite on a specific research question. Searches
multiple sources, compares their findings, documents contradictions and
consensus, and produces a structured review document.

Unlike `/research:investigate` (which drafts a comprehensive report on a broad topic),
`/research:review` answers a focused question by synthesizing evidence across sources.

## When to Use

- You have a specific, answerable question (not a broad topic)
- You need to compare what multiple sources say about the same question
- You want to document contradictions and consensus explicitly
- You need a defensible evidence base for a decision

---

## Workflow

### Standard Flow

```
User: /research:review "Does AI-driven pricing increase B2B customer churn?"
Claude: SYSTEMATIC REVIEW
        ====================

        RESEARCH QUESTION
        Does AI-driven pricing increase B2B customer churn?

        PICO Decomposition:
        - Population:    B2B customers
        - Intervention:  AI-driven pricing
        - Comparison:    Traditional pricing
        - Outcome:       Customer churn rates

        SEARCH STRATEGY
        ================
        Query variations:
        1. "AI pricing B2B customer churn"
        2. "dynamic pricing customer retention B2B"
        3. "algorithmic pricing B2B satisfaction"
        4. "automated pricing churn impact enterprise"

        Searching... (4 queries x 2 engines)

        SCREENING
        ==========
        Found: 18 results
        After deduplication: 14
        After title/abstract screening: 9
        After CRAAP evaluation (>= C): 7

        INCLUDED SOURCES (7)
        =====================
        | # | Source | Type | CRAAP | Finding |
        |---|--------|------|-------|---------|
        | 1 | McKinsey 2024 | Report | A | Churn decreased 12% |
        | 2 | Bain & Co 2025 | Report | A | No significant effect |
        | 3 | J. of Pricing 2024 | Academic | A+ | Churn increased 8% (SME) |
        | 4 | ... |

        SYNTHESIS
        =========

        CONSENSUS:
        - AI pricing improves retention for enterprise customers (3 sources agree)
        - Implementation quality matters more than the algorithm (4 sources)

        CONTRADICTIONS:
        - SME impact: Journal of Pricing finds +8% churn, McKinsey finds -5%
          Possible explanation: different market segments, sample sizes
        - Timeline: short-term churn spike vs long-term retention gain

        EVIDENCE GAPS:
        - No RCTs found — all observational or survey-based
        - No data on customers' perception of AI pricing fairness
        - Geographic bias: 6/7 sources are US-focused

        ANSWER TO RESEARCH QUESTION
        =============================
        MIXED — AI-driven pricing appears to reduce churn for enterprise
        customers but may increase it for SMEs. Evidence quality is moderate
        (no experimental studies). Implementation quality is a stronger
        predictor than the technology itself.

        Confidence: MODERATE (7 sources, no RCTs, some contradictions)

        Written: research/ai-pricing-b2b-churn-review.md

        Next steps:
        1. Use findings to inform /research:investigate on broader topic
        2. Or proceed directly with evidence for business decisions
```

---

## What the Skill Does

1. Read `.research/config.json` for targetPath
2. Parse the user's research question
3. **Decompose** using PICO framework (Population, Intervention, Comparison, Outcome)
4. **Generate search strategy:** 3-5 query variations per PICO element
5. **Search:** Use WebSearch with varied queries
6. **Screen:** Apply inclusion/exclusion process:
   - Deduplicate by URL
   - Title/snippet screening for relevance
   - CRAAP evaluation (reuses framework from `/research:investigate`)
7. **Fetch:** Use WebFetch to retrieve full content of included sources
8. **Extract findings:** For each included source, extract the finding relevant
   to the research question
9. **Synthesize:**
   - Identify consensus (3+ sources agree)
   - Identify contradictions (sources disagree)
   - Document possible explanations for contradictions
   - Flag evidence gaps
10. **Answer:** Provide a direct answer with confidence level
11. **Write:** Save as `research/<slug>-review.md`

---

## PICO Framework

The skill decomposes the research question into PICO elements to generate
comprehensive search queries.

| Element | Description | Example |
|---------|-------------|---------|
| **P**opulation | Who/what is being studied? | B2B customers |
| **I**ntervention | What is being done? | AI-driven pricing |
| **C**omparison | Compared to what? | Traditional pricing |
| **O**utcome | What is measured? | Customer churn |

Not all questions fit PICO perfectly. For non-comparative questions,
the skill adapts:

- **Descriptive:** "What is X?" → Population + Outcome only
- **Causal:** "Does X cause Y?" → Full PICO
- **Predictive:** "Will X happen?" → Population + Outcome + Timeframe

---

## Screening Process (PRISMA-Lite)

Inspired by PRISMA (Preferred Reporting Items for Systematic Reviews),
simplified for web-based research:

```
Identified (all search results)
    │
    ▼
Deduplicated (remove duplicate URLs)
    │
    ▼
Screened (title/snippet relevance check)
    │
    ▼
Evaluated (CRAAP framework, grade >= C)
    │
    ▼
Included (sources used in synthesis)
```

The flow diagram and counts are included in the review document for transparency.

---

## Synthesis Rules

### Consensus

A finding is marked as CONSENSUS when 3 or more independent sources agree
on the same conclusion (allowing for minor variation in numbers).

### Contradiction

A finding is marked as CONTRADICTION when 2 or more sources reach opposing
conclusions. For each contradiction, the skill documents:
- What each source claims
- Possible explanations (methodology, sample, geography, timeframe)
- Which source has higher evidence quality (CRAAP grade)

### Evidence Gaps

Gaps are flagged when:
- A PICO element has no coverage (no sources address Comparison, for example)
- All sources are the same evidence level (e.g., all industry reports, no academic)
- Geographic or temporal diversity is low (e.g., all US-based, all from one year)
- Key sub-questions remain unanswered

---

## Confidence Levels

| Level | Criteria |
|-------|----------|
| **HIGH** | 5+ sources, mostly consensus, at least one Level 1-3 evidence source |
| **MODERATE** | 3-5 sources, some contradictions, mostly Level 4-6 evidence |
| **LOW** | <3 sources, significant contradictions, or all Level 6-7 evidence |
| **INSUFFICIENT** | <2 credible sources, cannot draw conclusions |

---

## Review Document Format

```markdown
---
type: research-review
question: "Does AI-driven pricing increase B2B customer churn?"
status: complete
created: YYYY-MM-DD
confidence: moderate
sources_included: 7
sources_screened: 14
sources_identified: 18
---

# Systematic Review: [Question]

## Research Question

[Full question]

### PICO Decomposition

- **Population:** ...
- **Intervention:** ...
- **Comparison:** ...
- **Outcome:** ...

## Search Strategy

### Queries Used
1. "..."
2. "..."

### Screening Flow
- Identified: N
- Deduplicated: N
- Screened: N
- Evaluated: N
- Included: N

## Included Sources

| # | Source | Type | CRAAP | Finding |
|---|--------|------|-------|---------|
| 1 | ... | ... | ... | ... |

## Excluded Sources

| Source | Reason |
|--------|--------|
| ... | CRAAP below threshold |

## Synthesis

### Consensus
- [Finding with source references]

### Contradictions
- [Finding A] vs [Finding B]
  - Source 1 says: ...
  - Source 2 says: ...
  - Possible explanation: ...

### Evidence Gaps
- [Gap description]

## Answer

**[CONFIDENCE LEVEL]** — [Direct answer to the research question]

[Detailed explanation with qualifications]

## Bibliography

[Full citation list with CRAAP grades]
```

---

## Edge Cases

### Question too broad

```
Claude: Your question seems very broad: "How does AI affect business?"

        A systematic review works best with focused questions.

        Suggestions:
        1. "Does AI-driven customer service reduce support costs in SaaS?"
        2. "How does AI adoption affect employee productivity in SMEs?"

        Or use /research:investigate for broad topic exploration.
```

### Insufficient sources

```
Claude: Only 1 credible source found for this question.

        A systematic review requires multiple independent sources.

        Options:
        [B] Broaden the search terms
        [I] Switch to /research:investigate (single-source is ok for drafts)
        [A] Abort
```

### All sources agree (no contradictions)

```
Claude: Strong consensus — all 5 included sources agree.

        Note: Complete consensus can indicate selection bias.
        Consider whether dissenting views might exist in sources
        not captured by web search (e.g., industry-specific publications).
```

---

## Notes

- Review documents are standalone — they do NOT follow the research report lifecycle
  (no draft/verified status, no audit process)
- Reviews can feed into `/research:investigate` as a foundation for broader reports
- CRAAP evaluation is reused from the investigate skill
- The skill does not modify existing files
- Tool-agnostic: the review document is standard Markdown
