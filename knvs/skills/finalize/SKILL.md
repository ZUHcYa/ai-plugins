---
name: "finalize"
description: "Incorporate audit findings into a draft research report to produce verified status"
---

# /knvs:finalize

## Purpose

Incorporates a Maengelprotokoll (deficiency audit) into a draft research report.
Applies a strict editing protocol to transform draft claims into verified content.
The result is a research report with `status: verified`.

This skill does NOT generate new content. It only modifies, deletes, or precises existing text
based on external audit findings.

## When to Use

- After receiving a deficiency audit for a draft research report
- When `research/<slug>.md` (status: draft) AND `research/<slug>-audit.md` both exist
- Before `/knvs:impact` — finalization is a prerequisite for reliable impact extraction

## Workflow

### Standard Flow

```
User: /knvs:finalize
Claude: Scanning research/ for draft reports with audits...

        Found 1 report ready for finalization:

        1. [Projekt] Analysis [DRAFT]
           Draft:  research/projekt-analysis.md
           Audit:  research/projekt-analysis-audit.md
           Findings: 12 items (3 RED, 5 AMBER, 4 GREEN)

        Select report to finalize: [1]

User: 1
Claude: Loading draft and audit...

        EDITING PROTOCOL
        ================
        RED   (Hallucination/Not Verified) -> DELETE the claim
        AMBER (Context Missing/Nuance Lost) -> PRECISE the statement
        GREEN (Verified) -> KEEP unchanged

        Processing section by section...

        SECTION: Key Findings
        ---------------------

        Finding 1: "Market will reach $50B by 2028"
        Audit:     RED - Source not verifiable, link broken
        Action:    DELETE

        Finding 2: "Analyst firm predicts 15% of purchases by AI agents by 2028"
        Audit:     AMBER - Correct claim but missing report name and date
        Action:    PRECISE -> "Analyst firm predicts 15% of all B2B purchases
                   will be initiated by AI agents by 2028
                   (Report Title, October 2024)"

        Finding 3: "Major retailer already processes algorithmic orders"
        Audit:     GREEN - Verified via press release
        Action:    KEEP

        [... continues for all audit findings ...]

        FINALIZATION SUMMARY
        ====================
        Draft:    research/projekt-analysis.md
        Audit:    research/projekt-analysis-audit.md

        Applied:  12 findings
          RED:    3 claims deleted
          AMBER:  5 statements precised
          GREEN:  4 claims kept

        Structure: All required sections present
        Quellen-Export: Preserved (14 sources, 3 URLs removed for deleted claims)

        Frontmatter updated:
          status: draft -> verified
          verified: 2026-02-21

        -> Confirm finalization? [Y/n/review]

User: Y
Claude: research/projekt-analysis.md finalized.
        Status: VERIFIED (2026-02-21)

        Audit file: research/projekt-analysis-audit.md
        -> Keep for reference? [Y/delete]

User: delete
Claude: Audit file deleted.

        Next steps:
        - /knvs:impact to extract Impact Atoms
        - /knvs:ideate to create BMC from this research
```

### Review Before Confirm

```
User: review
Claude: [Shows the full finalized report with changes highlighted]

        Changes marked:
        - [DELETED] sections shown as strikethrough
        - [PRECISED] sections shown with before/after
        - [KEPT] sections unmarked

        -> Confirm finalization? [Y/n]
```

### Multiple Reports Ready

```
User: /knvs:finalize
Claude: Scanning research/ for draft reports with audits...

        Found 2 reports ready for finalization:

        1. [Projekt] Analysis [DRAFT]
           Audit: research/projekt-analysis-audit.md (12 findings)

        2. Fitness Market Study [DRAFT]
           Audit: research/fitness-market-audit.md (8 findings)

        Reports without audits:
        3. AI Commerce Trends [DRAFT] - No audit file found

        Select report to finalize: [1/2/all]
```

---

## What the Skill Does

1. Read `.knvs/config.json` for targetPath
2. Scan `research/` for files with `status: draft`
3. For each draft, check if a matching `<slug>-audit.md` file exists
4. If no argument: list draft reports with and without audits, user selects
5. Read both files fully: the draft report and the audit
6. Parse the audit into structured findings (see Audit Format below)
7. Process findings section by section, applying the editing protocol:
   - **RED findings:** Delete the claim from the draft. If deletion breaks paragraph
     coherence, rewrite the surrounding text to bridge the gap. If the deleted claim
     was a key structural element (e.g., a `### Finding N` heading with all content
     removed), remove the heading as well and renumber remaining findings.
   - **AMBER findings:** Rewrite the specific statement with added precision,
     qualifiers, date ranges, or source attribution as indicated in the audit.
   - **GREEN findings:** Leave text unchanged. Only fix typos or improve readability
     if the change is trivial and meaning-preserving.
8. After all findings processed, perform final polish pass:
   - Smooth text flow where deletions created gaps
   - Ensure section transitions read naturally
   - Preserve all required sections (Summary, Research Context, Key Findings,
     Supporting Data, Implications for Business Model, Next Steps)
   - Preserve Quellen-Export / Bibliography section: only remove URLs whose
     facts were completely deleted via RED findings
9. Present summary of all changes with counts by severity
10. On user confirmation: write the finalized report, update frontmatter
11. Update frontmatter: `status: verified`, add `verified: YYYY-MM-DD` (today's date)
12. Ask about audit file retention (keep or delete)

---

## Editing Protocol

The protocol is deterministic. The audit severity dictates the action — there is no
subjective judgment on what to do with each finding.

| Audit Severity | Label | Action | Scope |
|----------------|-------|--------|-------|
| **RED** | Hallucination / Not Verified | **DELETE** the claim entirely | Remove sentence or paragraph. Rewrite as hypothesis only if deletion breaks structural coherence. |
| **AMBER** | Context Missing / Nuance Lost | **PRECISE** the statement | Add specificity: dates, source names, qualifiers, geographic scope, sample sizes. Do not add new claims. |
| **GREEN** | Verified | **KEEP** unchanged | Only readability improvements: typo fixes, minor grammar. No semantic changes. |

### RED Exception: Hypothesis Rewrite

When a RED claim cannot simply be deleted because it is a structural anchor (e.g., the
only content under a `### Finding` heading, or the thesis of a paragraph), rewrite it
as an explicit uncertainty marker:

```markdown
<!-- Before (RED - deleted) -->
The market will reach $50B by 2028, driven by AI automation.

<!-- After (rewritten as uncertainty) -->
Industry estimates suggest significant market growth driven by AI automation,
though specific projections vary by source and cannot be independently verified.
```

This is a **last resort**. Prefer clean deletion with text flow repair.

### Quellen-Export Preservation

The Quellen-Export (or Bibliography) section at the end of the research report contains
raw source URLs and citations collected during external deep research. This section is
**never structurally modified** during finalization. It is the provenance record.

Only remove individual URLs/sources whose associated facts were completely deleted via RED findings.
If the draft has no Quellen-Export or Bibliography section, finalization proceeds normally.

---

## Audit Format (Maengelprotokoll)

### Expected Format

To ensure any AI producing the audit generates compatible output, the audit file
should follow this structure:

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
**Issue:** Source not verifiable, linked report does not exist
**Recommendation:** Delete claim entirely

---

### 2. [Section Name] - [Claim summary]

**Severity:** AMBER
**Location:** Key Findings > Finding 2
**Claim:** "Analyst firm predicts 15% of purchases by AI agents"
**Issue:** Missing report name, publication date, and exact scope (B2B vs. all)
**Recommendation:** Add report title, date, and scope qualifier

---

### 3. [Section Name] - [Claim summary]

**Severity:** GREEN
**Location:** Supporting Data > Market Statistics
**Claim:** "Major retailer processes 35% of orders via algorithmic systems"
**Issue:** None - verified via official press release (2024)
**Recommendation:** Keep as-is

---

## Overall Assessment

[Brief narrative: overall quality, main themes of deficiencies,
whether the report is close to verification or needs major rework]
```

### Tolerant Parsing

The audit may come from different AI systems with slight format variations.
The skill handles:
- German severity labels: ROT/GELB/GRUEN mapped to RED/AMBER/GREEN
- Emoji markers: 🔴/⚠️/✅ mapped to RED/AMBER/GREEN
- Missing `**Recommendation:**` field (for GREEN items, recommendation is implicit: keep)
- Unnumbered findings (use heading text for identification)
- Findings not wrapped in `### N.` headings (fall back to bold severity markers)

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

## Edge Cases

### Audit references a section that does not exist in the draft

```
Claude: Audit finding 7 references section "Technical Feasibility"
        but the draft has no such section.

        Options:
        [S] Skip this finding (section missing, nothing to fix)
        [A] Add section with audit recommendation as content

User: S
Claude: Finding 7 skipped.
```

### All findings are GREEN

```
Claude: All 8 audit findings are GREEN (verified).

        No content changes needed.
        Only updating frontmatter: status: draft -> verified

        -> Confirm? [Y/n]
```

### All findings are RED

```
Claude: Warning: All 6 audit findings are RED (hallucination/not verified).

        After deletion, the report may have insufficient content.

        Options:
        [P] Proceed with deletions (review result before saving)
        [A] Abort - the draft needs a new research pass, not finalization

User: A
Claude: Finalization aborted. The draft remains unchanged.
```

### Audit file has unexpected format

```
Claude: Warning: Audit file research/projekt-analysis-audit.md
        does not match expected Maengelprotokoll format.

        I found 0 parseable findings.

        Options:
        [M] Attempt manual parsing (I will show what I found)
        [A] Abort - please regenerate the audit in the expected format

        Tip: Use the Audit Generation Prompt from
        skills/finalize/SKILL.md > "Audit Format" section.
```

### Draft already has status: verified

```
Claude: research/projekt-analysis.md already has status: verified.

        Re-finalization is not supported. If you need to apply a new audit,
        manually set status back to "draft" first.
```

---

## Notes

- Finalization does NOT generate new content — it only modifies, deletes, or precises existing text
- The skill never adds claims, statistics, or sources not present in the draft or audit recommendations
- Quellen-Export / Bibliography is preserved as the provenance record
- The audit file is a one-time input — it is not tracked in frontmatter or referenced downstream
- Multiple finalization passes are not supported — once verified, the report is immutable
- Tool-agnostic: the user can perform finalization manually by reading the audit and applying RED/AMBER/GREEN rules in any Markdown editor
- The Maengelprotokoll format is a recommendation, not a hard requirement — the skill attempts tolerant parsing
