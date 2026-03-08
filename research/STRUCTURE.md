# research Folder Structure

Overview of how research files are organized on disk.

---

## Path Structure

research uses a flat folder for all reports and audits:

```
<dataDir>/
├── machine-customers.md
├── machine-customers-audit.md
├── machine-customers-evaluation.md
├── saas-pricing.md
├── ai-pricing-churn-review.md
└── ...
```

**Example with `dataDir: "./research"`:**

```
research/
├── machine-customers.md              # Research report (draft or verified)
├── machine-customers-audit.md        # External audit (Maengelprotokoll)
├── machine-customers-evaluation.md   # Self-critique (/research:evaluate output)
├── saas-pricing.md                   # Another research report
└── ai-pricing-churn-review.md        # Systematic review (/research:review output)
```

---

## Why This Structure

| Decision | Reason |
|----------|--------|
| Flat folder (no subfolders) | Simple, greppable, works in any file browser |
| kebab-case slug | Consistent, URL-safe, sortable |
| No date prefix or numbering | Reports identified by topic, not order |
| Audit files alongside reports | Audit is always `<slug>-audit.md` next to `<slug>.md` |

---

## File Naming

| Type | Pattern | Example |
|------|---------|---------|
| Research Report | `<slug>.md` | `machine-customers.md` |
| Audit File | `<slug>-audit.md` | `machine-customers-audit.md` |
| Evaluation File | `<slug>-evaluation.md` | `machine-customers-evaluation.md` |
| Systematic Review | `<slug>-review.md` | `ai-pricing-churn-review.md` |

---

## Research Report Format

```yaml
---
type: research
title: "Machine Customers"
status: verified
created: 2026-02-22
verified: 2026-02-22
source_report: "original-filename.pdf"  # optional
vendor: "Company Name"                   # optional
---

# Research Report: Machine Customers

## Summary
[Overview of findings]

## Research Context
### Problem Space
### Market Context
### Target Audience

## Key Findings
### Finding 1: [Title]
**Sources:**
- [Citation]

## Supporting Data
## Implications for Business Model
## Next Steps
```

**YAML frontmatter is the single source of truth** for structured metadata.

---

## Required vs. Optional Fields (Frontmatter)

| Field | Required | Notes |
|-------|----------|-------|
| `type` | Yes | Always `research` |
| `title` | Yes | Human-readable title |
| `status` | Yes | `draft` or `verified` |
| `created` | Yes | YYYY-MM-DD |
| `verified` | When verified | YYYY-MM-DD of verification |
| `source_report` | No | Reference to original document |
| `vendor` | No | External organization name |

---

## Audit File Format

```yaml
---
type: research-audit
target: "research/<slug>.md"
auditor: "Claude Opus / Extended Thinking"
audit_date: 2026-02-22
summary:
  red: 3
  amber: 5
  green: 4
---

# Maengelprotokoll: [Title]

## Findings
### 1. [Section] - [Claim]
**Severity:** RED | AMBER | GREEN
**Location:** Section > Subsection
**Claim:** "Exact quote"
**Issue:** What is wrong
**Recommendation:** Fix
```

---

## Configuration

**Location:** `.research/config.json`

```json
{
  "dataDir": "./research"
}
```

---

## Evaluation File Format

```yaml
---
type: research-evaluation
target: "research/<slug>.md"
evaluator: "Claude / research:evaluate"
evaluation_date: 2026-03-08
scores:
  logical_soundness: 6
  bias_awareness: 4
  evidence_quality: 5
  argument_structure: 3
  overall: 4.5
---

# Evaluation: [Title]

## Logical Fallacies
## Bias Indicators
## Unbacked Claims
## Argument Structure (Toulmin)
## Summary
```

---

## Systematic Review Format

```yaml
---
type: research-review
question: "Does X cause Y?"
status: complete
created: 2026-03-08
confidence: moderate
sources_included: 7
sources_screened: 14
sources_identified: 18
---

# Systematic Review: [Question]

## Research Question
## Search Strategy
## Included Sources
## Excluded Sources
## Synthesis (Consensus / Contradictions / Evidence Gaps)
## Answer
## Bibliography
```

---

**Version:** 1.1.0
