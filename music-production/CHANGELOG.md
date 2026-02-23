# Changelog

All notable changes to music-production will be documented in this file.

---

## [0.5.1] - 2026-02-23

### Fixed

- **CLAUDE.md** — Added missing Versioning section (was the only plugin without it)
- **`/music-production:produce`** — Fixed duplicate step number 6 in "What the Skill Does" (renumbered 6-13)
- **`/music-production:generate`** — Fixed duplicate step number 6 in "What the Skill Does" (renumbered 7-8)

---

## [0.5.0] - 2026-02-21

### Added

- New skill: `/music-production:strategy` - guided setup for artist identity, distribution strategy, and production philosophy
- Persistent strategy document (`.music-production/strategy.md`) with machine-readable frontmatter
- Covers 5 blocks: Artist Identity, Distribution, Constraints, Hardware & Visual, Release Cycle
- Re-runnable: update individual sections or start fresh

### Changed

- `/music-production:start` now detects `strategy.md` and shows artist name in status header (or hints at strategy setup)
- `/music-production:produce` loads strategy context for identity-aware consultation (genre constraints, hardware-specific advice, anti-genre filtering)
- `/music-production:generate` applies distribution rules from strategy (metadata syntax, tag filtering, cover art keywords, artist name in footer)
- `/music-production:release` now optionally collects Video and Artwork status, appending dedicated sections to the song file
- SessionStart hook shows `artist_name` from strategy in the status line

---

## [0.4.0] - 2026-02-21

### Added

- New skill: `/music-production:produce` - advisory production companion for songs in progress
- Covers 6 focus areas: Arrangement, Harmony & Theory, Sound Design, Rhythm & Groove, Mixing, Mastering
- Song-linked mode (reads song file for context) and free consultation mode
- Guiding Questions tables per focus area (usable as standalone manual checklist)
- Optional writing of action items to song TODOs and consultation notes to Notizen

---

## [0.3.0] - 2026-02-20

### Added

- Initial plugin release: song lifecycle management from idea to catalog
- 6 skills: `/music-production:start`, `/music-production:new`, `/music-production:vault`, `/music-production:release`, `/music-production:generate`, `/music-production:catalog`
- Smart entry point: auto-detects first run vs. returning user
- Catalog numbering system with configurable prefix (e.g. `FW001`, `FW002`)
- Song lifecycle: `01_Projects/` → `02_Vault/` → Release-Prep → `03_Catalog/`
- Central artifact: one Markdown file per song, named after its folder, grows from minimal notes to full release document
- Config stored in `.music-production/config.json` (catalog prefix, next number)
- Tool-agnostic design: all files are plain Markdown — works in any editor
