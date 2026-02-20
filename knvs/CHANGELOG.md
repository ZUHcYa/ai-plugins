# Changelog

All notable changes to knvs will be documented in this file.

---

## [0.13.0] - 2026-02-20

### Changed

- **BREAKING:** Impact callouts now include a direction symbol prefix in the title: `▼` (Threat), `▲` (Opportunity), `▼▲` (Both)
- **Severity Assessment** renamed to **Severity & Direction Assessment** in `/knvs:ideate` — direction is proposed together with severity in a single step
- Direction is context-specific (same impact can be ▼ Threat for one model and ▲ Opportunity for another) and is NOT stored in the Impact Atom
- Hypothesis backlink wording updated to "Severity + Direction based on hypothesis: ..."

### Migration

Existing callouts without a direction symbol remain valid. Add the symbol during the next `/knvs:ideate` or `/knvs:review` session on the affected canvas.

---

## [0.12.0] - 2026-02-20

### Changed

- All documentation translated from German to English
- Removed stale `/knvs:verify` references from QUICKSTART.md
- Fixed repository references from `ai-hub` to `ai-plugins`
- Added `reviews/` to QUICKSTART.md setup output example
- Added `network/` folder to README.md file structure
- Updated STRUCTURE.md version footer to 0.12.0

---

## [0.11.0] - 2026-02-18

### Removed

- **`/knvs:verify` skill**: Removed. The use case (correcting AI-generated research with deficiencies) no longer fits the core workflow. Users now bring already externally verified research papers.
- **Research Report Template from `verify/SKILL.md`**: Template migrated to `CLAUDE.md` under "Research Report: Structure".
- **verify-specific frontmatter fields**: `deficiency_list` and `corrections_applied` removed from required fields.

### Changed

- **`/knvs:impact`**: Now also accepts research with `status: draft` (previously only `verified`). Hypotheses remain excluded (`status: hypothesis` → not available).
- **`/knvs:ideate`**: "From research" mode now also shows draft reports (previously only verified). Hypotheses still excluded.
- **`/knvs:start`**: Verify removed from phase lifecycle. Suggested Actions: draft research now shows `/knvs:impact` instead of `/knvs:verify`. First-run messages updated.
- **`/knvs:sync`**: "Stale Driver" check downgraded from error to advisory (draft as impact driver is now legitimate).
- **CLAUDE.md**: `status: verified` can be set manually. Frontmatter required fields simplified. Hypothesis lifecycle formulated without `/knvs:verify`.
- **Version**: 0.10.0 → 0.11.0

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

- **BREAKING:** Impact callouts are now embedded inline under BMC dimensions instead of a separate `## Impact Context` section
- Inline impacts use Obsidian callout syntax (`> [!impact]`) — degrades to blockquote in other Markdown viewers
- Field-specific reasoning: each callout only shows the pressure on the relevant dimension

### Added

- Inline callout generation in `/knvs:ideate` (automatic when creating canvas from research with impacts)
- `/knvs:sync` detects deprecated `## Impact Context` format and orphaned inline impact links

### Removed

- `## Impact Context` section from canonical IDEATE canvas template

### Migration

- **New canvases:** Automatically inline format via `/knvs:ideate`
- **Existing canvases:** Continue to work, `/knvs:sync` warns optionally
- **Manual migration:** Move impacts as callouts under affected dimensions, delete old section

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

- **sync-config skill**: Removed (only existed for source-tracking infrastructure)
- **config-schema.json**: Custom convention removed (not used by Claude Code)
- **config-manager utility**: Orphaned `.claude-plugin/utils/config-manager/` removed
- **Verbosity system**: quiet/normal/verbose output levels removed from all skills
- **Output guidelines boilerplate**: Identical sections removed from 6 skills (already covered in CLAUDE.md)
- **WebFetch version-check**: Network dependency removed from `/knvs:start`
- **Config v3.0 infrastructure**: Source-tracking, migration chains, `ui.verbosity` removed

### Changed

- **Config simplified**: From v3.0 (8 fields) to flat JSON `{ "targetPath": "./" }` (analogous to trips)
- **Setup logic inline**: config-manager pseudo-API references replaced with direct instructions
- **`/knvs:review`**: CLI flags (`--list`, `--view`) replaced with natural language

### Why

ADR-008 compliance: Plugin = instruction layer, no infrastructure.
Same cleanup as trips v0.4.0.

---

## [0.5.0] - 2026-02-14

### Changed

- Version reset: All plugins set to realistic 0.x versions
- Previous versions (1.0-2.1) did not reflect actual maturity
- Version display introduced in `/knvs:start` header

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
