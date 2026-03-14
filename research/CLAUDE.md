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
| Research Report | `[slug].md` | `machine-customers.md` |
| Audit File | `[slug]-audit.md` | `machine-customers-audit.md` |
| Evaluation File | `[slug]-evaluation.md` | `machine-customers-evaluation.md` |
| Systematic Review | `[slug]-review.md` | `ai-pricing-churn-review.md` |

**Rule:** NO date prefix. NO numbering. kebab-case slug only.

---

## Research Report: Structure

Research reports follow a draft-to-verified lifecycle.

**Required fields (frontmatter):**
- `type: research`
- `title`
- `status` (draft | verified)
- `created`
- `verified` (YYYY-MM-DD, when status=verified)

**Optional fields:**
- `source_report` (reference to the original document)
- `vendor` (external organization name)

**Required Sections:**
Summary, Research Context, Key Findings (with sources), Supporting Data,
Implications for Business Model, Next Steps.

**Finalization workflow:**
1. External AI generates draft (`status: draft`)
2. Separate AI session creates a Maengelprotokoll (`<slug>-audit.md`)
3. `/research:finalize` incorporates audit findings and sets `status: verified`

Full report template is in `skills/investigate/SKILL.md`.

---

## Audit Format (Maengelprotokoll)

**Tolerant Parsing:** The audit may come from different AI systems. The finalize skill handles:
- German severity labels: ROT/GELB/GRUEN mapped to RED/AMBER/GREEN
- Emoji markers mapped to RED/AMBER/GREEN
- Missing fields, unnumbered findings, flexible heading structure

Full audit format and generation prompt are in `skills/finalize/SKILL.md`.

---

## Cross-Plugin Integration: vntrs

Verified research reports can be consumed by the **vntrs** plugin (separate plugin, separate repo).
The integration is unidirectional: research produces, vntrs reads — research has no dependency on vntrs.

**What vntrs reads:**
- Reports with `status: verified` from `research/` folder
- Section `## Implications for Business Model`
- Referenced via `source_research:` frontmatter field in vntrs hypothesis files

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/research:` prefix | `/research:start`, `/research:investigate` |
| Skill files | `<skill>/SKILL.md` | `start/SKILL.md`, `finalize/SKILL.md` |
| Configuration | `.research/` folder | `.research/config.json` |

---

## Tool-Agnostic: Manual Workflow Always Possible

All files are pure Markdown with standard frontmatter. Skills are helpers, not a prerequisite.
Users can create, edit, and verify files manually. No proprietary formats, no tool-specific dependencies.
