# Changelog

All notable changes to knvs will be documented in this file.

---

## [7.0.0] - 2026-03-14

### Breaking Changes

- **Decoupled from Strategyzer methodology** — Plugin is now methodology-agnostic
- **`type: bmc` → `type: canvas`** — Canvas type is now generic. Optional `canvas_type: bmc` for BMC canvases. `type: bmc` accepted for backward compatibility
- **`bmc_fields` → `canvas_fields`** — Renamed across all artifact types. `bmc_fields` accepted as fallback
- **Hypothesis categories** — Now free-text instead of D/F/V enum. Desirability/Feasibility/Viability remain as default suggestions for BMC canvases
- **Confidence levels simplified** — 4 levels → 3: removed `very-low` (mapped to `low` when reading)
- **Decision terminology** — `persevere` → `continue`, `kill` → `stop`. Old values mapped when reading
- **Canvas validation** — "Missing BMC Fields" health check → "Canvas has no content sections". Canvases must have at least 1 `##` section (not necessarily 9 BMC fields)
- **Strategyzer Loop** → **Innovation Loop** — All references updated
- **Attribution** — Strategyzer CC BY-SA footer → "Based on Business Model Canvas by Osterwalder/Pigneur"

### Changed

- `CLAUDE.md` rewritten (796 → ~350 lines): templates moved to skills, assessment dimensions moved to assess/SKILL.md
- All skills updated for new terminology (Continue/Pivot/Stop, canvas_fields, type: canvas)
- `README.md` rewritten: merged QUICKSTART.md, removed Strategyzer references
- `plugin.json` description and keywords updated

### Removed

- `QUICKSTART.md` — content merged into README.md
- Hardcoded 9-field BMC requirement — canvases now self-describe via `##` headings
- Strategyzer footer from canvas templates (replaced with Osterwalder/Pigneur CC BY-SA note)

### Compatibility

- `type: bmc` continues to work (mapped to `type: canvas`)
- `bmc_fields` continues to work (mapped to `canvas_fields`)
- `confidence: very-low` mapped to `low` when reading
- `decision: persevere` mapped to `continue`, `kill` mapped to `stop` when reading
- Existing D/F/V category values remain valid (free-text accepts any string)

---

## [6.0.0] - 2026-03-04

### Breaking Changes

- **Canvas-First folder structure** — All artifacts (hypotheses, experiments, insights, learning-cards, assessments) now live inside the canvas folder instead of separate top-level type folders. `hypotheses/<canvas-slug>/` → `explore/<canvas-slug>/hypotheses/`, etc.
- **Canvas file naming** — Canvas files are now `<phase>/<canvas-slug>/<canvas-slug>.md` instead of `<phase>/<canvas-slug>.md`
- **Canvas-relative paths** — All intra-canvas frontmatter references are relative to the canvas folder (e.g., `hypothesis: hypotheses/slug.md` instead of `hypotheses/canvas-slug/slug.md`)
- **`canvas:` field** — Now contains only the canvas slug (e.g., `canvas: ai-bookkeeping`) instead of a full path (`canvas: explore/ai-bookkeeping.md`)
- **`pivot_from:` field** — Now contains only the original canvas slug (e.g., `pivot_from: original`) instead of a path
- **Setup creates 3 folders** — `/knvs:start` now creates only `explore/`, `exploit/`, `archive/`. Canvas subfolders (hypotheses/, experiments/, etc.) are created on-demand by respective skills
- **Health Check #9 replaced** — "Orphaned Data Folder" check (impossible in Canvas-First) replaced by "Canvas folder without canvas file" check

### Why Canvas-First

- Artifacts belong to exactly one canvas — folder structure now reflects the domain model
- Kill/Archive: one `mv` operation instead of 5-6 separate moves across type folders
- EXPLORE→EXPLOIT: `mv explore/slug/ exploit/slug/` — canvas-relative references stay valid
- Pivot: `mv explore/slug/ archive/slug/` + create new canvas folder

### Migration

1. For each canvas in `explore/`: create `explore/<slug>/` folder, move `explore/<slug>.md` → `explore/<slug>/<slug>.md`
2. Move `hypotheses/<slug>/*.md` → `explore/<slug>/hypotheses/`
3. Move `experiments/<slug>/*.md` → `explore/<slug>/experiments/`
4. Move `insights/<slug>/*.md` → `explore/<slug>/insights/`
5. Move `learning-cards/<slug>/*.md` → `explore/<slug>/learning-cards/`
6. Repeat for `exploit/` canvases (including `assessments/<slug>/` → `exploit/<slug>/assessments/`)
7. Update frontmatter in all artifacts: `canvas:` → slug only, all paths → canvas-relative
8. Delete empty top-level type folders (`hypotheses/`, `experiments/`, `insights/`, `learning-cards/`, `assessments/`)

---

## [5.1.0] - 2026-03-04

### Breaking Changes

- **Assessment: `assessment_type` field removed** — Type is derived from filename suffix (`-performance` / `-trend`). No separate frontmatter field needed.
- **Assessment: `date` field removed** — Date is derived from filename prefix (`YYYY-MM-DD-...`). No separate frontmatter field needed.
- **Assessment: `## Date` section removed** — Redundant with filename. Assessment files now start with `## Scores`.
- **Learning Card (Experiment): `result` field removed** — Was a 1:1 copy of the experiment's `result` field. The card links to the experiment via `experiment:` — look up the result there.
- **Learning Card (Hypothesis): `confidence` field removed** — Was a 1:1 copy of the hypothesis's `confidence` field. The card links to the hypothesis via `hypothesis:` — look up the confidence there.
- **Experiment: `success_criteria` frontmatter field removed** — Redundant with the `## Success Criteria` section in the experiment body, which provides more detail. Success criteria are now exclusively documented in the section.

### Migration

1. Assessment files: Remove `assessment_type:` and `date:` from frontmatter, remove `## Date` section
2. Learning Card (experiment scope): Remove `result:` from frontmatter
3. Learning Card (hypothesis scope): Remove `confidence:` from frontmatter
4. Experiment files: Remove `success_criteria:` from frontmatter (keep `## Success Criteria` section as-is)

---

## [5.0.0] - 2026-03-04

### Breaking Changes

- **`innovation_risk` field removed** — Replaced by unified `risk` field (`high | moderate | low`) present in ALL canvases (EXPLORE and EXPLOIT)
- **`potential_revenue` field removed** — Replaced by unified `revenue` field (`high | moderate | low`) present in ALL canvases
- **Health Check #4 changed** — Now checks for `risk` field instead of `innovation_risk`, scans both `explore/` and `exploit/`

### Added

- **Unified `risk` field** — Same frontmatter field in EXPLORE and EXPLOIT canvases. EXPLORE: derived from hypothesis validation state (updated by `/knvs:learn`). EXPLOIT: derived from Performance + Trend scores (updated by `/knvs:assess`). Can be manually set/overridden in any Markdown editor
- **Unified `revenue` field** — Expected revenue potential (`high | moderate | low`) across both phases. User-set estimate, carried over at EXPLORE-to-EXPLOIT transition (no longer dropped)
- **Risk derivation in `/knvs:learn`** — After hypothesis updates, computes and updates canvas `risk` based on importance-weighted hypothesis validation state
- **Risk derivation in `/knvs:assess`** — After assessment, computes and updates canvas `risk` from combined Performance + Trend scores
- **Risk-aware portfolio view** — `/knvs:start` now shows `risk`, `revenue`, and P/T scores for EXPLOIT canvases instead of only "last assessed" date
- **Score-based indicators** — TESTING and EXPLOIT priority indicators based on `risk` field (red=high, yellow=moderate, green=low) instead of assessment recency only
- **Risk-based suggested actions** — EXPLOIT canvases with `risk:high` trigger urgent review suggestions

### Migration

1. Replace `innovation_risk: [any value]` with `risk: high` (or appropriate level) in EXPLORE canvas frontmatter
2. Replace `potential_revenue: [any value]` with `revenue: high|moderate|low` in canvas frontmatter
3. Add `risk: high` and `revenue:` to EXPLOIT canvas frontmatter (risk will be updated by next `/knvs:assess` run)
4. Add `risk: high` and `revenue:` to any EXPLORE draft canvases missing these fields

---

## [4.0.4] - 2026-02-27

### Fixed

- **/knvs:experiment backlink** — After creating an experiment, the skill now adds the experiment link to `## Related Experiments` in the hypothesis file (was: section always stayed empty at `(none yet)`)

---

## [4.0.3] - 2026-02-27

### Fixed

- **/knvs:hypothesize interaction** — Added explicit one-at-a-time interaction rule: skill now presents hypotheses individually and waits for user approval (`Y/n/edit`) before proceeding to the next (was: all hypotheses output at once)

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
