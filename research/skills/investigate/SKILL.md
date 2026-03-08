---
name: "investigate"
description: "Structured web research on a topic with source evaluation and draft report generation"
---

# /research:investigate

## Purpose

Conducts structured web research on a given topic. Searches multiple sources,
evaluates each source's credibility using the CRAAP framework, and produces a
draft research report with inline citations and source quality assessments.

This skill bridges the gap between "I need to research X" and "I have a draft report."

## When to Use

- Starting research on a new topic
- Need a structured, source-evaluated draft before external audit
- Want to replace ad-hoc web searches with a documented research process

---

## Workflow

### Standard Flow

```
User: /research:investigate machine customers B2B
Claude: Starting structured investigation...

        RESEARCH QUESTION
        ==================
        "What are machine customers in B2B contexts?"

        Refined sub-questions:
        1. What defines a machine customer?
        2. What is the current market size and growth trajectory?
        3. Which industries are adopting machine customers?
        4. What are the key technical enablers?
        5. What are the risks and limitations?

        Searching... (5 queries)

        SOURCES FOUND (12)
        ===================
        Evaluating credibility (CRAAP framework)...

        | # | Source | Type | CRAAP | Keep? |
        |---|--------|------|-------|-------|
        | 1 | Gartner Report 2025 | Industry Report | A | Yes |
        | 2 | HBR Article | Expert Analysis | A | Yes |
        | 3 | Random blog post | Blog | D | No — low authority |
        | 4 | IEEE Paper | Academic | A+ | Yes |
        | ... |

        Kept 8 of 12 sources (4 filtered by quality threshold).

        EVIDENCE SYNTHESIS
        ===================
        Mapping claims to evidence...

        Writing draft report...

        DRAFT COMPLETE
        ==============
        Written: research/machine-customers-b2b.md (status: draft)

        Report contains:
        - 8 sources (CRAAP-evaluated)
        - 14 claims with evidence mapping
        - 3 evidence gaps flagged

        Next steps:
        1. Review the draft manually
        2. Run external audit → /research:finalize
        3. Or run /research:evaluate for self-critique
```

---

## What the Skill Does

1. Read `.research/config.json` for targetPath
2. Parse the user's topic into a research question
3. Decompose the question into 3-7 sub-questions for thorough coverage
4. **Search phase:** Use WebSearch to find sources for each sub-question
   - Run 2-3 searches per sub-question with varied query formulations
   - Collect URLs, titles, and snippets
5. **Fetch phase:** Use WebFetch to retrieve full content of promising sources
   - Skip paywalled or inaccessible content gracefully
6. **Evaluate phase:** Apply CRAAP framework to each source (see below)
   - Filter out sources below quality threshold (grade D or F)
7. **Classify evidence:** Assign each source an evidence hierarchy level
8. **Synthesize:** Build a structured draft report:
   - Map every claim to its supporting evidence
   - Flag claims with only single-source support
   - Flag evidence gaps where sub-questions lack good answers
   - Include CRAAP grades in the Bibliography section
9. **Write draft:** Save as `research/<slug>.md` with `status: draft`
10. Provide summary with source counts, claim counts, and evidence gaps

---

## CRAAP Framework

Every source is evaluated on five dimensions. Each dimension scores 1-5.
The composite grade determines whether the source is kept.

| Dimension | Question | Score Guide |
|-----------|----------|-------------|
| **Currency** | When was this published/updated? | 5 = <1 year, 4 = 1-2 years, 3 = 2-5 years, 2 = 5-10 years, 1 = >10 years |
| **Relevance** | Does this directly address the research question? | 5 = directly, 3 = tangentially, 1 = barely related |
| **Authority** | Who published this? What are their credentials? | 5 = peer-reviewed/official, 3 = industry expert, 1 = anonymous/unknown |
| **Accuracy** | Is it supported by evidence? Are claims verifiable? | 5 = data-backed with citations, 3 = plausible but unverified, 1 = contradicts known facts |
| **Purpose** | Why does this exist? Is there bias? | 5 = informational/educational, 3 = mixed purpose, 1 = promotional/advocacy |

### Composite Grades

| Grade | Score Range | Action |
|-------|------------|--------|
| A+ | 23-25 | Keep — excellent source |
| A | 19-22 | Keep — strong source |
| B | 15-18 | Keep — acceptable source, note limitations |
| C | 11-14 | Marginal — include only if no better source exists |
| D | 7-10 | Exclude — too unreliable |
| F | 5-6 | Exclude — not credible |

### Quality Threshold

Default: grade C or above (score >= 11). Sources below threshold are excluded
from the draft but listed in a "Excluded Sources" appendix for transparency.

---

## Evidence Hierarchy

When multiple sources make conflicting claims, higher-evidence sources take precedence.

| Level | Type | Weight |
|-------|------|--------|
| 1 (highest) | Systematic Review / Meta-Analysis | Strongest |
| 2 | Randomized Controlled Trial (RCT) | Strong |
| 3 | Cohort Study / Longitudinal Study | Moderate |
| 4 | Case-Control Study | Moderate |
| 5 | Industry Report / Market Analysis | Moderate (context-dependent) |
| 6 | Expert Opinion / Commentary | Weak |
| 7 | Anecdotal / Case Study (single) | Weakest |

For business/technology research (most common use case), Level 5 (Industry Reports)
is often the highest available evidence. This is acceptable — the hierarchy informs
relative weighting, not absolute exclusion.

---

## Draft Report Format

The generated draft follows the standard research report format from CLAUDE.md,
with these additions:

### Claim-Evidence Mapping

Each key finding includes explicit evidence mapping:

```markdown
### Finding 1: Machine customers will represent 20% of B2B transactions by 2028

**Evidence:**
- Gartner (2025): "Machine customers... 20% of all revenue by 2028" [CRAAP: A, Level 5]
- McKinsey (2024): "Automated procurement... 15-25% by 2028" [CRAAP: A, Level 5]

**Confidence:** MODERATE (two industry sources agree on range, no academic validation)
```

### Evidence Gaps

Flagged at the end of Key Findings:

```markdown
### Evidence Gaps

- No academic research found on machine customer adoption rates
- Conflicting data on SME vs enterprise adoption timelines
- No longitudinal studies on machine customer satisfaction metrics
```

### Bibliography with CRAAP Grades

```markdown
## Bibliography

| # | Source | Type | CRAAP | Evidence Level |
|---|--------|------|-------|----------------|
| 1 | Gartner, "Machine Customers 2025" | Industry Report | A (21/25) | 5 |
| 2 | Smith et al., IEEE 2024 | Academic Paper | A+ (24/25) | 3 |
| ... |

### Excluded Sources (below threshold)

| Source | CRAAP | Reason |
|--------|-------|--------|
| BlogName.com | D (9/25) | Low authority, promotional content |
```

---

## Slug Generation

The report slug is derived from the research topic:
- Lowercase, kebab-case
- Max 40 characters
- No date prefix (per CLAUDE.md convention)

Example: "Machine Customers B2B" → `machine-customers-b2b`

---

## Edge Cases

### No sources found

```
Claude: No credible sources found for this topic.

        Options:
        [R] Retry with broader search terms
        [M] Create empty draft with research question documented
        [A] Abort
```

### All sources below quality threshold

```
Claude: Found 6 sources, but none meet quality threshold (CRAAP >= C).

        Best source: BlogPost (score: 10/25, grade D)

        Options:
        [L] Lower threshold to D (include with warnings)
        [R] Retry with different search terms
        [A] Abort
```

### Report slug already exists

```
Claude: research/machine-customers-b2b.md already exists (status: draft).

        Options:
        [O] Overwrite existing draft
        [N] Use different slug: machine-customers-b2b-2
        [A] Abort
```

---

## Limitations

- Web search quality depends on available WebSearch tool
- Cannot access paywalled academic databases (PubMed abstracts only)
- CRAAP scoring is heuristic, not definitive
- Evidence hierarchy is simplified for general research
- This skill generates drafts — external audit via `/research:finalize` is still recommended

---

## Notes

- The skill produces a DRAFT, never a verified report
- External audit remains the path to verification
- CRAAP grades are included in the report for audit transparency
- All claims are mapped to evidence for traceability
- Evidence gaps are explicitly flagged, not hidden
- Tool-agnostic: the report can be reviewed in any Markdown editor
