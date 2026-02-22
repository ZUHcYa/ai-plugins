---
name: "finalize"
description: "Incorporate audit findings into a draft research report to produce verified status"
---

# /research:finalize

## Purpose

Incorporates a Maengelprotokoll (deficiency audit) into a draft research report.
Applies a strict editing protocol to transform draft claims into verified content.
The result is a research report with `status: verified`.

This skill does NOT generate new content. It only modifies, deletes, or precises existing text
based on external audit findings.

## When to Use

- After receiving a deficiency audit for a draft research report
- When `research/<slug>.md` (status: draft) AND `research/<slug>-audit.md` both exist

## Workflow

### Standard Flow

```
User: /research:finalize
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

        [... shows each finding with action ...]

        FINALIZATION SUMMARY
        ====================
        Applied:  12 findings
          RED:    3 claims deleted
          AMBER:  5 statements precised
          GREEN:  4 claims kept

        Frontmatter updated:
          status: draft -> verified
          verified: 2026-02-22

        -> Confirm finalization? [Y/n/review]

User: Y
Claude: research/projekt-analysis.md finalized.
        Status: VERIFIED (2026-02-22)

        Audit file: research/projekt-analysis-audit.md
        -> Keep for reference? [Y/delete]
```

---

## What the Skill Does

1. Read `.research/config.json` for targetPath
2. Scan `research/` for files with `status: draft`
3. For each draft, check if a matching `<slug>-audit.md` file exists
4. If no argument: list draft reports with and without audits, user selects
5. Read both files fully: the draft report and the audit
6. Parse the audit into structured findings (see Audit Format in CLAUDE.md)
7. Process findings section by section, applying the editing protocol:
   - **RED findings:** Delete the claim from the draft. If deletion breaks paragraph
     coherence, rewrite the surrounding text to bridge the gap. If the deleted claim
     was a key structural element, remove the heading as well and renumber.
   - **AMBER findings:** Rewrite the specific statement with added precision,
     qualifiers, date ranges, or source attribution as indicated in the audit.
   - **GREEN findings:** Leave text unchanged. Only fix typos or improve readability
     if the change is trivial and meaning-preserving.
8. After all findings processed, perform final polish pass:
   - Smooth text flow where deletions created gaps
   - Ensure section transitions read naturally
   - Preserve all required sections
   - Preserve Quellen-Export / Bibliography section: only remove URLs whose
     facts were completely deleted via RED findings
9. Present summary of all changes with counts by severity
10. On user confirmation: write the finalized report, update frontmatter
11. Update frontmatter: `status: verified`, add `verified: YYYY-MM-DD` (today's date)
12. Ask about audit file retention (keep or delete)

---

## Editing Protocol

The protocol is deterministic. The audit severity dictates the action.

| Audit Severity | Label | Action | Scope |
|----------------|-------|--------|-------|
| **RED** | Hallucination / Not Verified | **DELETE** the claim entirely | Remove sentence or paragraph. Rewrite as hypothesis only if deletion breaks structural coherence. |
| **AMBER** | Context Missing / Nuance Lost | **PRECISE** the statement | Add specificity: dates, source names, qualifiers, geographic scope, sample sizes. Do not add new claims. |
| **GREEN** | Verified | **KEEP** unchanged | Only readability improvements: typo fixes, minor grammar. No semantic changes. |

### RED Exception: Hypothesis Rewrite

When a RED claim cannot simply be deleted because it is a structural anchor, rewrite it
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
raw source URLs and citations. This section is **never structurally modified** during finalization.

Only remove individual URLs/sources whose associated facts were completely deleted via RED findings.

---

## Edge Cases

### All findings are GREEN

```
Claude: All 8 audit findings are GREEN (verified).
        No content changes needed.
        Only updating frontmatter: status: draft -> verified
        -> Confirm? [Y/n]
```

### All findings are RED

```
Claude: Warning: All 6 audit findings are RED.
        After deletion, the report may have insufficient content.

        Options:
        [P] Proceed with deletions (review result before saving)
        [A] Abort - the draft needs a new research pass, not finalization
```

### Audit file has unexpected format

```
Claude: Warning: Audit file does not match expected format.
        I found 0 parseable findings.

        Options:
        [M] Attempt manual parsing (I will show what I found)
        [A] Abort - please regenerate the audit in the expected format

        Tip: Use the Audit Generation Prompt from CLAUDE.md.
```

### Draft already has status: verified

```
Claude: research/projekt-analysis.md already has status: verified.
        Re-finalization is not supported. If you need to apply a new audit,
        manually set status back to "draft" first.
```

---

## Notes

- Finalization does NOT generate new content
- The skill never adds claims, statistics, or sources not present in the draft or audit
- Quellen-Export / Bibliography is preserved as the provenance record
- Multiple finalization passes are not supported
- Tool-agnostic: the user can perform finalization manually in any Markdown editor
