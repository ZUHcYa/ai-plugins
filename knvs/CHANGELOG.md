# Changelog

All notable changes to knvs will be documented in this file.

---

## [4.0.2] - 2026-02-27

### Fixed

- **Strategyzer attribution footer** — Added `© Strategyzer AG — strategyzer.com — CC BY-SA 3.0` footer to all BMC canvas templates (draft and scaling) to comply with CC BY-SA 3.0 license requirements

---

## [4.0.1] - 2026-02-23

### Fixed

- **EXPLOIT canvas template** — Removed stale `revenue_score` field, added `performance_score`, `trend_score`, `last_assessment` (expected by `/knvs:assess` and `/knvs:start` health checks)
- **Trend Assessment dimension 4** — +3 statement was copy-pasted from dimension 3 (Customer Friction). Corrected to Social/Cultural-specific statement in both `assess/SKILL.md` and `CLAUDE.md`
- **`/knvs:learn` result ownership** — Removed duplicate `result` field write (already set by `/knvs:capture`). Removed undocumented "Partial success" from status update logic
- **Kill flow orphan prevention** — Kill now moves related subfolders (`hypotheses/`, `experiments/`, `insights/`, `learning-cards/`, `assessments/`) to `archive/` to prevent persistent Health Check #9 warnings
- **Namespace Strategy table** — Now lists all 9 skills (was 4)
- **Decision Log labels** — Changed from German to English for language consistency
- **Folder Structure** — Added cross-plugin read note for `research/` dependency

---

## [4.0.0] - 2026-02-23

### Breaking Changes

- **`/knvs:decide` skill removed** — Hypothesis Learning Cards in `/knvs:card` fully replace the decide skill. Cards now include Persevere/Pivot/Kill decisions with full action execution (archive, pivot)
- **Learning Card `scope` field** — New required field distinguishes `experiment` (existing) and `hypothesis` (new) cards

### Added

- **Hypothesis Learning Cards** in `/knvs:card` [H] mode — conclude the testing cycle of one hypothesis with aggregated evidence, synthesized insights, and a strategic decision (Persevere/Pivot/Kill)
- **Hypothesis Dashboard** shown during card creation (same as former decide dashboard)
- **`scope`** frontmatter field on learning cards (`experiment` | `hypothesis`)
- **`decision`** frontmatter field on hypothesis cards (`persevere` | `pivot` | `kill`)
- **`experiments`** frontmatter field (array) on hypothesis cards

### Changed

- **`/knvs:card`** — Mode selection: [E] Experiment Card, [H] Hypothesis Card
- Experiment cards get `scope: experiment` field (backwards compatible — cards without `scope` are treated as experiment)

### Migration

1. Existing learning cards continue to work (no `scope` field = experiment card)
2. Replace `/knvs:decide` usage with `/knvs:card` [H] mode
3. Existing decision log entries in canvases remain valid

---

## [3.2.0] - 2026-02-23

### Breaking Changes

- **`evidence` field replaced by `confidence`** on hypothesis frontmatter — 4-level Strategyzer Hypothesis Confidence model (`very-low | low | moderate | high`) replaces 3-level evidence model (`none | weak | strong`)
- **From-Research hypotheses** now start with empty `confidence` instead of `evidence: weak` — confidence is derived from own experiments only, `source_research` tracks external context

### Changed

- **`/knvs:learn`** — New Confidence Level Update Logic: assesses confidence based on ALL completed experiments (count + evidence_strength + experiment_type), recommends level, user confirms
- **`/knvs:hypothesize`** — Template uses `confidence:` (empty) instead of `evidence: none`
- **`/knvs:decide`** — Decision dashboard shows confidence level per hypothesis
- **`/knvs:experiment`** — Priority display uses confidence instead of evidence
- **Prioritization** — `importance x confidence` replaces `importance x evidence`

### Migration

1. Replace `evidence: none` with `confidence:` (empty) in hypothesis frontmatter
2. Replace `evidence: weak` with `confidence: very-low` or `confidence: low` (based on experiment data)
3. Replace `evidence: strong` with `confidence: moderate` or `confidence: high` (based on experiment data)
4. `evidence_strength` on experiment files is unchanged

---

## [3.1.0] - 2026-02-23

### Added

- **NEW SKILL:** `/knvs:card` — Create standalone Learning Cards from completed experiments. Synthesizes hypothesis (We believed), experiment results (We observed), insights (We learned), and next action (Therefore we will) into the classic "Testing Business Ideas" 4-part format
- **`learning-cards/`** folder for Learning Cards, grouped by canvas slug
- **Health Check #11** — Validates Learning Card `experiment` references point to existing experiment files

---

## [3.0.0] - 2026-02-23

### Breaking Changes

- **`/knvs:review` skill removed** — Trend Assessment in `/knvs:assess` fully replaces Disruption Reviews (10 quantitative dimensions vs. 5 qualitative areas, same methodology source)
- **`reviews/` folder removed** from structure
- **`next_review` frontmatter field** replaced by `next_assessment`
- **`disruption_risk` frontmatter field** removed (Trend Score provides quantitative equivalent)

### Migration

1. Existing review files in `reviews/` can be kept as archive reference
2. Replace `next_review` with `next_assessment` in EXPLOIT canvas frontmatter
3. Remove `disruption_risk` field from EXPLOIT canvas frontmatter
4. Use `/knvs:assess` (Trend mode) instead of `/knvs:review`

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
