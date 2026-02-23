# research: Plugin-Specific Rules

## Vision

**research** is a standalone plugin for structured research workflows. It manages the lifecycle
of research reports: from draft through external audit to verified status.

This plugin is tool-agnostic and works with any Markdown editor.

### Strict Rules

- **Development support only** (code gen, docs, tests, automation)
- **No automatic deployments** without review

### Versioning

`CHANGELOG.md` is the single source of truth for the version. `plugin.json` is synced automatically via git pre-commit hook — never edit it manually.

| Change type | Version bump |
|-------------|-------------|
| New feature, Breaking change in Skills | Minor (`x.y.0`) |
| Bugfix, docs correction | Patch (`x.y.z`) |

Always add a `## [x.y.z] - YYYY-MM-DD` entry to `CHANGELOG.md` before committing.

---

## File Naming Conventions

| Type | Format | Example |
|-----|--------|---------|
| Research Report | `[slug].md` | `machine-customers-verified.md` |
| Audit File | `[slug]-audit.md` | `machine-customers-audit.md` |

**Rule:** NO date prefix. NO numbering. kebab-case slug only.

---

## Research Report: Structure

Research reports are structured analyses of external topics (markets, technologies,
competitors, trends). They follow a draft-to-verified lifecycle.

**Required fields (frontmatter):**
- `type: research`
- `title`
- `status` (draft | verified)
- `created`
- `verified` (YYYY-MM-DD, when status=verified)

**Optional fields:**
- `source_report` (reference to the original document)
- `vendor` (name of the external organization, when the report focuses on a specific vendor, partner, or competitor)

**Note:** `status: verified` can be set manually when the research report has already been
reviewed externally. The `verified` date field is also required for manual verification
(enter your own review date).

**Finalization workflow:** For AI-generated research drafts, the recommended path is:
1. External AI generates draft (`status: draft`)
2. Separate AI session creates a Maengelprotokoll (`research/<slug>-audit.md`)
3. `/research:finalize` incorporates audit findings and sets `status: verified`

**Required Sections:**
- Summary
- Research Context (Problem Space, Market Context, Target Audience)
- Key Findings (with sources)
- Supporting Data
- Implications for Business Model
- Next Steps

**Optional Sections:**
- Limitations
- Bibliography

### Research Report Template

```markdown
---
type: research
title: "[Research Topic]"
status: verified
created: YYYY-MM-DD
verified: YYYY-MM-DD
source_report: "[Original filename]"  # optional
---

# Research Report: [Title]

## Summary

[Overview of the research findings]

---

## Research Context

### Problem Space
[What problem does this research address?]

### Market Context
[Market size, trends, dynamics]

### Target Audience
[Who is affected by this problem?]

---

## Key Findings

### Finding 1: [Title]
[Content]

**Sources:**
- [Citation 1 with link]

---

## Supporting Data

### Market Statistics
[Statistics with sources]

### Competitive Landscape
[Competitive analysis]

### Technical Feasibility
[Technical assessments]

---

## Implications for Business Model

[Assessments of how this research could inform a business model]

---

## Next Steps

- [ ] Review and use findings in relevant context
```

---

## Audit Format (Maengelprotokoll)

### Expected Format

```markdown
---
type: research-audit
target: "research/<slug>.md"
auditor: "Claude Opus / Extended Thinking"
audit_date: YYYY-MM-DD
summary:
  red: 3
  amber: 5
  green: 4
---

# Maengelprotokoll: [Research Title]

**Target:** [[research/<slug>]]
**Audit Date:** YYYY-MM-DD

---

## Findings

### 1. [Section Name] - [Claim summary]

**Severity:** RED
**Location:** Key Findings > Finding 1
**Claim:** "[Exact quote from draft]"
**Issue:** What is wrong
**Recommendation:** What to do

---

## Overall Assessment

[Brief narrative assessment]
```

### Tolerant Parsing

The audit may come from different AI systems with slight format variations.
The skill handles:
- German severity labels: ROT/GELB/GRUEN mapped to RED/AMBER/GREEN
- Emoji markers: red_circle/warning/white_check_mark mapped to RED/AMBER/GREEN
- Missing `**Recommendation:**` field
- Unnumbered findings
- Findings not wrapped in `### N.` headings

### Audit Generation Prompt

*Ready to copy into Claude Opus / Extended Thinking session:*

````
You are a research auditor. Review the following draft research report
with engineering-grade rigor.

For EVERY factual claim, statistic, quote, and source citation in the report,
determine one of three severities:

- RED (Hallucination / Not Verified): Claim cannot be verified from
  the cited source, source does not exist, or claim is fabricated.
- AMBER (Context Missing / Nuance Lost): Claim is broadly correct but
  missing important context, qualifiers, dates, or attribution specifics.
- GREEN (Verified): Claim is accurate and properly attributed.

Output format:
1. YAML frontmatter with type: research-audit, target, auditor, audit_date,
   and summary counts (red, amber, green).
2. Numbered findings (### 1. Section - Claim), each with:
   - **Severity:** RED | AMBER | GREEN
   - **Location:** Section > Subsection
   - **Claim:** "Exact quote from draft"
   - **Issue:** What is wrong or missing
   - **Recommendation:** Specific fix
3. Overall Assessment paragraph at the end.

Here is the draft report to audit:

[PASTE DRAFT HERE]
````

---

## Warning-Signal Check

**Before every task, ask:**
1. Does this help with **developing research** (code, docs, automation)?
2. Is it **not a user feature** for end users?

**If in doubt:** IMMEDIATELY ask the developer!

---

## Emoji Usage

**Rule:** Emojis only when they provide real value - not as decoration.

### Allowed
- Status indicators where text alone would be unclear (e.g. in compact tables)

### Forbidden
- Decorative emojis without function
- Multiple emojis per line/heading
- Emojis in body text

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/research:` prefix | `/research:start`, `/research:finalize` |
| Skill files | `<skill>/SKILL.md` | `start/SKILL.md`, `finalize/SKILL.md` |
| Configuration | `.research/` folder | `.research/config.json` |

---

## Tool-Agnostic: Manual Workflow Always Possible

**Core principle:** The user must always be able to carry out the research process **manually without skills**.

**Rules:**
- All files are pure Markdown with standard frontmatter
- Skills are helpers, not a prerequisite
- Users can create, edit, and verify files manually
- Folder structure and file format are self-explanatory

**Forbidden:**
- Obsidian-specific plugins or Dataview queries as core functionality
- Proprietary file formats
- Features that only work with Claude Code
- Dependencies on external APIs or services
