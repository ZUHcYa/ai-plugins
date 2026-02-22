# research Folder Structure

Overview of how research files are organized on disk.

---

## Path Structure

research uses a flat folder for all reports and audits:

```
<dataDir>/
├── machine-customers.md
├── machine-customers-audit.md
├── saas-pricing.md
└── ...
```

**Example with `dataDir: "./research"`:**

```
research/
├── machine-customers.md
├── machine-customers-audit.md
└── saas-pricing.md
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

**Version:** 1.0.0
