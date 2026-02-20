# Changelog

All notable changes to knvs will be documented in this file.

---

## [0.11.0] - 2026-02-18

### Removed

- **`/knvs:verify` skill**: Entfernt. Der Use Case (KI-generierte Research mit Maengeln korrigieren) entspricht nicht mehr dem Kernworkflow. Nutzer bringen bereits extern verifizierte Research Paper mit.
- **Research Report Template aus `verify/SKILL.md`**: Template nach `CLAUDE.md` unter "Research Report: Struktur" migriert.
- **verify-spezifische Frontmatter-Felder**: `deficiency_list` und `corrections_applied` aus den Pflichtfeldern entfernt.

### Changed

- **`/knvs:impact`**: Akzeptiert jetzt auch Research mit `status: draft` (vorher nur `verified`). Hypothesen bleiben ausgeschlossen (`status: hypothesis` → nicht verfuegbar).
- **`/knvs:ideate`**: "From research" Modus zeigt jetzt auch Draft-Reports (vorher nur verified). Hypothesen weiterhin ausgeschlossen.
- **`/knvs:start`**: Verify aus Phase-Lifecycle entfernt. Suggested Actions: Draft-Research zeigt `/knvs:impact` statt `/knvs:verify`. First-Run-Messages aktualisiert.
- **`/knvs:sync`**: "Stale Driver" Check von Error zu Advisory herabgestuft (Draft als Impact-Driver ist jetzt legitim).
- **CLAUDE.md**: `status: verified` kann manuell gesetzt werden. Frontmatter-Pflichtfelder vereinfacht. Hypothesen-Lifecycle ohne `/knvs:verify` formuliert.
- **Version**: 0.10.0 -> 0.11.0

---

## [0.10.0] - 2026-02-17

### Added

- **Hypothesis capture pattern**: Unverified market assumptions can be captured as research briefs (`status: hypothesis`) during any skill interaction or ad-hoc
- **Hypothesis file format**: Lightweight template with `claim`, `bmc_fields`, optional `origin_impact` in `research/` folder
- **Severity-dispute trigger in `/knvs:ideate`**: When user disagrees with severity based on an unverified assumption, offers to capture it as a hypothesis with backlink in callout
- **Hypothesis references in `/knvs:review` and `/knvs:impact`**: One-liner references to centralized hypothesis pattern
- **5 new `/knvs:sync` checks**: Stale hypothesis, orphaned hypothesis, resolved hypothesis, invalid hypothesis format, missing claim field
- **`/knvs:start` hypothesis display**: Blue indicator for hypotheses, suggested actions for stale and resolved hypotheses

### Changed

- **`/knvs:impact`**: Shows `[HYPOTHESIS]` instead of `[DRAFT]` for hypothesis files in report listing
- **Version**: 0.9.0 -> 0.10.0

---

## [0.9.0] - 2026-02-17

### Changed

- **BREAKING:** Impact Callouts werden jetzt inline unter BMC-Dimensionen eingebettet statt in separater `## Impact Context` Sektion
- Inline Impacts nutzen Obsidian-Callout-Syntax (`> [!impact]`) — degradiert zu Blockquote in anderen Markdown-Viewern
- Feldspezifische Begruendung: Jeder Callout zeigt nur den Druck auf die jeweilige Dimension

### Added

- Inline Callout-Generierung in `/knvs:ideate` (automatisch bei Canvas-Erstellung aus Research mit Impacts)
- `/knvs:sync` erkennt deprecated `## Impact Context` Format und verwaiste Inline-Impact-Links

### Removed

- `## Impact Context` Sektion aus kanonischem IDEATE Canvas Template

### Migration

- **Neue Canvases:** Automatisch inline Format via `/knvs:ideate`
- **Bestehende Canvases:** Funktionieren weiter, `/knvs:sync` warnt optional
- **Manuelle Migration:** Impacts als Callouts unter betroffene Dimensionen verschieben, alte Sektion loeschen

---

## [0.8.0] - 2026-02-17

### Added

- **`/knvs:impact` skill**: Extract atomic impacts from verified research
  - Batch extraction with individual review per impact
  - Duplicate detection across existing impacts
  - Multi-driver support for convergent findings
  - Maps each impact to affected BMC fields
- **`impacts/` folder**: Staging area for impact atoms between research and ideation
- **Impact Atom template**: Structured format with severity, tags, BMC field mapping

### Changed

- **`/knvs:start`**: Creates `impacts/` folder, shows impact status in portfolio
- **`/knvs:ideate`**: Impact-aware canvas creation, surfaces related impacts during BMC generation
- **`/knvs:review`**: Impact Challenge mode (ALL vs NEW since last review)
- **`/knvs:sync`**: Scans `impacts/` folder, checks for orphaned/invalid impact atoms
- **Version**: 0.7.0 -> 0.8.0

### Documentation

- Updated CLAUDE.md with "Impact Atom: Struktur" section
- Updated STRUCTURE.md with `impacts/` folder and lifecycle
- Updated README.md with `/knvs:impact` skill documentation
- Updated QUICKSTART.md with optional impact extraction step

---

## [0.7.0] - 2026-02-16

### Added

- **`/knvs:verify` skill**: Pre-ideate research verification
  - Corrects hallucinated claims from external research
  - Fixes wrong source citations
  - Addresses logical gaps
  - Uses WebSearch for active fact-checking (German and English sources)
  - Non-verifiable claims removed and documented under Limitations
  - Produces corrected research reports with correction log
  - Claude Opus recommended for maximum analysis depth
- **`research/` folder**: Staging area for pre-ideate research reports
- **Research report template**: Structured format for verified research

### Changed

- **`/knvs:start`**: Creates `research/` folder, shows research status in portfolio
- **`/knvs:ideate`**: Can create BMC from verified research reports
- **`/knvs:sync`**: Scans `research/` folder, checks for stale/orphaned reports
- **Version**: 0.6.0 → 0.7.0

### Documentation

- Updated STRUCTURE.md with `research/` folder and lifecycle
- Updated CLAUDE.md with research report requirements
- Updated README.md with `/knvs:verify` skill documentation
- Updated QUICKSTART.md with optional research verification step

---

## [0.6.0] - 2026-02-16

### Removed

- **sync-config skill**: Entfernt (existierte nur fuer Source-Tracking-Infrastruktur)
- **config-schema.json**: Custom-Convention entfernt (nicht von Claude Code genutzt)
- **config-manager Utility**: Verwaiste `.claude-plugin/utils/config-manager/` entfernt
- **Verbosity-System**: quiet/normal/verbose Output-Level aus allen Skills entfernt
- **Output Guidelines Boilerplate**: Identische Sections aus 6 Skills entfernt (bereits in CLAUDE.md abgedeckt)
- **WebFetch Version-Check**: Netzwerk-Abhaengigkeit aus /knvs:start entfernt
- **Config v3.0 Infrastruktur**: Source-Tracking, Migration-Ketten, ui.verbosity entfernt

### Changed

- **Config vereinfacht**: Von v3.0 (8 Felder) auf flat JSON `{ "targetPath": "./" }` (analog trips)
- **Setup-Logik inline**: config-manager Pseudo-API-Referenzen durch direkte Anweisungen ersetzt
- **/knvs:review**: CLI-Flags (`--list`, `--view`) durch Natural Language ersetzt

### Why

ADR-008 Compliance: Plugin = Instruktionsschicht, keine Infrastruktur.
Gleicher Cleanup wie trips v0.4.0.

---

## [0.5.0] - 2026-02-14

### Changed

- Version reset: Alle Plugins auf realistische 0.x Versionen zurueckgesetzt
- Vorherige Versionen (1.0-2.1) spiegelten nicht den tatsaechlichen Reifegrad wider
- Versionsanzeige in /knvs:start Header eingefuehrt

---

## [2.1.0] - 2026-02-06

### Removed

- **`examples/` directory**: Filled example canvases removed (not needed for workflow)
- **`templates/` directory**: Templates migrated into skills as `## Canvas Template` sections (Single Source of Truth)
- **`commands/` directory**: Redundant with skills, which serve as both slash commands and auto-triggered skills

### Changed

- Canvas templates are now the canonical definition inside each SKILL.md
- `/knvs:start` generates template files from skill definitions on first run
- `plugin.json` now includes `version` field
- Fixed stale references to removed skills (`/knvs:portfolio`, `/knvs:setup`)
- Updated STRUCTURE.md, README.md, QUICKSTART.md to reflect new structure

### Migration Guide

**From 2.0 to 2.1:**

1. Your existing canvases are not affected
2. `templates/` and `examples/` folders can be deleted from your workspace
3. Canvas templates are now in `skills/*/SKILL.md` under `## Canvas Template`

---

## [2.0.0] - 2026-01-30

### Breaking Changes

- **Plugin Structure**: knvs is now a proper Claude Code plugin
  - Added `.claude-plugin/plugin.json` manifest
  - Skills moved from `skills/knvs-*.md` to `skills/*/SKILL.md` format
  - Skills now use YAML frontmatter for metadata

- **Removed Team Mode**: Simplified to single-user workflow
  - Removed `--team` / `--solo` flags from `/knvs:setup`
  - Removed Git integration from setup
  - Removed team moderation from `/knvs:review`
  - Config no longer has `mode` and `git` fields

- **Removed npm CLI**: No longer distributed via npm
  - Removed `npx knvs init` command
  - Removed `knvs update`, `knvs check`, `knvs backup` commands
  - Installation now via Git clone or GitHub template

### Changed

- `/knvs:setup` now only asks for target folder and verbosity
- `/knvs:sync` uses file timestamps only (no Git diff)
- `/knvs:review` is solo-only (no `--team` flag)
- Simplified config structure in `.knvs/config.json`

### Migration Guide

**From 1.x to 2.0:**

1. Your existing canvases in `ideate/`, `explore/`, `exploit/` are preserved
2. Update your `.knvs/config.json`:
   ```json
   {
     "version": "2.0",
     "created": "YYYY-MM-DD",
     "targetPath": "./",
     "ui": {
       "verbosity": "normal"
     }
   }
   ```
3. Old `mode` and `git` fields are ignored

---

## [1.2.0] - Previous

- Added output verbosity levels (quiet/normal/verbose)
- Added custom target folder support
- Various skill improvements

## [1.1.0] - Previous

- Added `/knvs:sync` skill
- Added disruption reviews with separate files
- Added portfolio dashboard

## [1.0.0] - Initial Release

- Initial knvs release
- IDEATE -> EXPLORE -> EXPLOIT workflow
- Basic skills: setup, ideate, explore, exploit, review, portfolio
