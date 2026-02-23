# Changelog

All notable changes to knvs will be documented in this file.

---

## [2.5.0] - 2026-02-23

### Added

- **NEW SKILL:** `/knvs:capture` — Start experiments (designed → running) and document results (running → completed). Bridges the gap between `/knvs:experiment` (design) and `/knvs:learn` (insights)
- **`evidence_strength`** frontmatter field on experiments (`weak | strong`) — reflects data quality independent of result. Set by `/knvs:capture` during result documentation

---

## [2.4.1] - 2026-02-23

### Fixed

- Correct assessment descriptions: Performance = current snapshot (Ist-Aufnahme), Trend = future projection (Zukunftsprojektion). Removed incorrect axis labels.

---

## [2.4.0] - 2026-02-22

### Added

- **Trend Assessment** mode in `/knvs:assess` — 10 future-oriented dimensions (-3 to +3) for Death & Disruption Risk (future projection). Based on "The Invincible Company"
- **`trend_score`** frontmatter field for EXPLOIT canvases

### Changed

- `/knvs:assess` now offers mode selection: **[P] Performance** or **[T] Trend**
- Assessment filenames now include type suffix: `YYYY-MM-DD-performance.md`, `YYYY-MM-DD-trend.md`

---

## [2.3.0] - 2026-02-22

### Added

- **`/knvs:assess`** — Performance Assessment for EXPLOIT canvases. 10 dimensions (-3 to +3), total Performance Score (-30 to +30). Based on "The Invincible Company"
- **`assessments/`** folder for assessment history, grouped by canvas slug
- **`performance_score`** and **`last_assessment`** frontmatter fields for EXPLOIT canvases

---

## [2.2.1] - 2026-02-22

### Changed

- Clarify: hypothesis loop (hypothesize → experiment → learn → decide) only applies to Explore BMCs, not Exploit. Exploit BMCs have their own mechanisms.

---

## [2.2.0] - 2026-02-22

### Added

- **`/knvs:hypothesize`** — "From Research" mode: generate hypotheses from verified research reports against an existing BMC. Evidence starts at `weak` (audited source exists)
- **`source_research`** — Optional frontmatter field for hypotheses derived from external research
- **Health Check #10** — Validates `source_research` references point to existing verified reports

---

## [2.1.0] - 2026-02-22

### Changed

- **`/knvs:start`** — Now includes Health Checks section (schema validation, reference integrity, folder consistency)

### Removed

- **`/knvs:sync` skill** — All valuable checks consolidated into `/knvs:start` Health section. File-timestamp tracking dropped (required state file)

---

## [2.0.0] - 2026-02-22

### Breaking Changes

- **`ideate/` folder removed** — `/knvs:ideate` now writes directly to `explore/` with `status: draft`
- **`/knvs:explore` skill removed** — No more file moving between folders. User sets `status: testing` manually when ready
- **`/knvs:ideas` skill removed** — `/knvs:start` shows drafts in portfolio overview
- **Status model simplified** — `explore/`: draft | testing | validated. `exploit/`: scaling
- **`progress` field removed** — Replaced by status values (draft = WIP, testing = active)

### Changed

- **`/knvs:ideate`** — Creates BMC in `explore/` with `status: draft` instead of `ideate/` with `status: IDEATE`
- **`/knvs:start`** — Portfolio groups by DRAFTS / TESTING / EXPLOIT instead of IDEATE / EXPLORE / EXPLOIT
- **`/knvs:decide`** — Pivot creates new canvas in `explore/` (was `ideate/`)
- **`/knvs:exploit`** — Sets `status: scaling` (was `status: EXPLOIT`)
- **`/knvs:sync`** — Updated consistency checks for new status model
- **`/knvs:hypothesize`** — Lists all `explore/` canvases regardless of status
- **All docs** — Updated for 2-folder structure (explore/ + exploit/)

### Removed

- `ideate/` folder (replaced by `explore/` with `status: draft`)
- `/knvs:explore` skill (status change replaces file move)
- `/knvs:ideas` skill (redundant with `/knvs:start` portfolio)
- `progress` frontmatter field (replaced by status values)
- `READY FOR EXPLORE` concept

### Migration

**From 1.0.0 to 2.0.0:**

1. Move files from `ideate/` to `explore/` and set `status: draft`
2. Update canvas frontmatter: replace `status: IDEATE` with `status: draft`, remove `progress` field
3. Update canvas frontmatter: replace `status: EXPLORE` with `status: testing`
4. Update canvas frontmatter: replace `status: EXPLOIT` with `status: scaling`
5. Delete empty `ideate/` folder

---

## [1.0.0] - 2026-02-22

### Breaking Changes

- **COMPLETE RESTRUCTURING** — Plugin realigned with Strategyzer "Testing Business Ideas" / "The Invincible Company" methodology
- **Research pipeline removed** — Extracted into standalone `research` plugin. `research/`, `impacts/`, `network/` folders no longer part of knvs
- **Impact Atoms removed** — Replaced by Hypothesis → Experiment → Insight loop
- **Network Canvas removed** — Vendor/partner tracking no longer part of knvs

### Added

- **Design-Boundary** — Strategyzer-Loop as mandatory check for all future features
- **NEW SKILL:** `/knvs:hypothesize` — Extract and prioritize Desirability/Feasibility/Viability hypotheses from BMC
- **NEW SKILL:** `/knvs:experiment` — Design experiments with predefined types (Customer Interview, Landing Page, Concierge MVP, etc.) and custom types
- **NEW SKILL:** `/knvs:learn` — Extract insights from completed experiments, update hypothesis evidence levels
- **NEW SKILL:** `/knvs:decide` — Persevere/Pivot/Kill decision with evidence dashboard and Decision Log
- **Hypothesis data model** — Individual files with evidence/importance frontmatter for 2x2 prioritization
- **Experiment data model** — Predefined + custom experiment types from "Testing Business Ideas"
- **Insight data model** — Standalone, reusable learning files
- **Decision Log** — `## Decisions` section in canvas for Persevere/Pivot/Kill history
- **Pivot mechanics** — New canvas with `pivot_from` reference, original archived
- **`archive/` folder** — For killed/pivoted canvases
- **New folders:** `hypotheses/`, `experiments/`, `insights/`, `archive/`

### Removed

- **`/knvs:impact` skill** — Impact Atoms concept removed
- **`/knvs:finalize` skill** — Moved to `research` plugin as `/research:finalize`
- **Impact Atoms** — `impacts/` folder, impact-atom type, inline impact callouts
- **Network Canvas** — `network/` folder, network-canvas type, vendor-risk callouts
- **3-Tier Signal System** — Replaced by hypothesis-based validation
- **Impact Challenge Mode** in `/knvs:review`
- **Research Report Structure** — Moved to `research` plugin
- **Hypothesis-as-Research** — Old format (type: research, status: hypothesis) replaced by new hypothesis data model

### Changed

- **`/knvs:ideate`** — Simplified: no impact atoms, no vendor-risk, added `## Decisions` section, `pivot_from` support
- **`/knvs:explore`** — No longer generates hypotheses inline. Points to `/knvs:hypothesize` as next step
- **`/knvs:exploit`** — Reads hypothesis status from `hypotheses/` folder instead of inline canvas sections
- **`/knvs:review`** — Removed Impact Challenge Mode
- **`/knvs:sync`** — New consistency checks for hypotheses, experiments, insights. Removed impact/network/research checks
- **`/knvs:start`** — Updated lifecycle diagram, folder structure, suggested actions for hypothesis/experiment workflow
- **`/knvs:ideas`** — Minimal: removed impact atom references
- **CLAUDE.md** — Complete rewrite with Strategyzer-Loop, new data models, Design-Boundary

### Migration

**From 0.14.0 to 1.0.0:**

1. Existing canvases in `ideate/`, `explore/`, `exploit/` are preserved
2. `research/`, `impacts/`, `network/` folders should be managed by the new `research` plugin
3. Inline `[!impact]` callouts in canvases can be removed or kept as reference
4. Hypotheses should be re-created using `/knvs:hypothesize`
5. Inline `## Hypotheses` sections in EXPLORE canvases are replaced by files in `hypotheses/`

---

## [0.14.0] - 2026-02-21

### Added

- **NEW SKILL:** `/knvs:finalize` — incorporates external Maengelprotokoll into draft research reports
  - Strict editing protocol: RED (delete), AMBER (precise), GREEN (keep)
  - Tolerant audit parsing (German labels, emoji markers, flexible structure)
  - Includes copyable Audit Generation Prompt for extended-thinking sessions
  - 2 new sync checks: orphaned audits + draft-with-audit ready
  - Suggested action in `/knvs:start` for draft+audit pairs

---

## [0.13.0] - 2026-02-20

### Changed

- **BREAKING:** Impact callouts now include a direction symbol prefix in the title: ▼ (Threat), ▲ (Opportunity), ▼▲ (Both)
- **Severity Assessment** renamed to **Severity & Direction Assessment** in `/knvs:ideate`

---

## [0.12.0] - 2026-02-20

### Changed

- All documentation translated from German to English

---

## [0.11.0] - 2026-02-18

### Removed

- **`/knvs:verify` skill**: Removed

### Changed

- `/knvs:impact`: Now also accepts `status: draft`
- `/knvs:ideate`: "From research" mode now also shows draft reports

---

## [0.10.0] - 2026-02-17

### Added

- Hypothesis capture pattern

---

## [0.9.0] - 2026-02-17

### Changed

- **BREAKING:** Impact callouts now inline under BMC dimensions

---

## [0.8.0] - 2026-02-17

### Added

- `/knvs:impact` skill and Impact Atoms system

---

## [0.7.0] - 2026-02-16

### Added

- `/knvs:verify` skill and `research/` folder

---

## [0.6.0] - 2026-02-16

### Removed

- sync-config, config-schema, config-manager, verbosity system

---

## [0.5.0] - 2026-02-14

- Version reset to realistic 0.x versions

---

## [0.1.0] - Initial Release

- Initial knvs release with IDEATE → EXPLORE → EXPLOIT workflow
