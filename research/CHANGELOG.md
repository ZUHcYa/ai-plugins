# Changelog

All notable changes to research will be documented in this file.

---

## [1.1.1] - 2026-03-14

### Changed

- `CLAUDE.md`: Removed duplicated templates (report, audit, CRAAP, evidence hierarchy ~180 lines) — these live in skills
- Consolidated `QUICKSTART.md` into `README.md`

### Removed

- `QUICKSTART.md` — content merged into README.md

---

## [1.1.0] - 2026-03-08

### Added

- **`/research:investigate`** — Structured web research with CRAAP source evaluation
  - WebSearch + WebFetch integration for multi-source research
  - CRAAP framework (Currency, Relevance, Authority, Accuracy, Purpose) grades every source
  - Evidence hierarchy awareness (Meta-Analysis > RCT > Cohort > Expert Opinion)
  - Claim-evidence mapping with confidence levels and evidence gap flagging
  - Output: draft research report ready for audit cycle
- **`/research:evaluate`** — Critical thinking analysis of research reports
  - Logical fallacy detection (10 types: Appeal to Authority, Hasty Generalization, etc.)
  - Cognitive bias scanning (Confirmation, Selection, Survivorship, Anchoring, etc.)
  - Unbacked claim identification
  - Toulmin argument model analysis (Claim, Data, Warrant, Qualifier, Rebuttal)
  - Dimensional scoring (Logical Soundness, Bias Awareness, Evidence Quality, Argument Structure)
- **`/research:review`** — Systematic review lite for focused research questions
  - PICO question decomposition (Population, Intervention, Comparison, Outcome)
  - PRISMA-lite screening process with deduplication and quality filtering
  - Multi-source synthesis: consensus, contradictions, and evidence gaps
  - Confidence levels (HIGH/MODERATE/LOW/INSUFFICIENT)
  - Standalone review documents (no draft/verified lifecycle)

---

## [1.0.1] - 2026-02-23

### Fixed

- **CLAUDE.md** — Added missing Warning-Signal Check and Emoji Usage sections for consistency with all other plugins

---

## [1.0.0] - 2026-02-22

### Added

- **Initial release** — Research plugin extracted from knvs plugin
- **`/research:start`** — Smart entry point: setup on first run, overview afterwards
- **`/research:finalize`** — Incorporate audit findings into draft research reports
  - Strict editing protocol: RED (delete), AMBER (precise), GREEN (keep)
  - Tolerant audit parsing (German labels, emoji markers, flexible structure)
  - Includes copyable Audit Generation Prompt for extended-thinking sessions
- Research report data model with draft/verified lifecycle
- Audit format (Maengelprotokoll) specification
