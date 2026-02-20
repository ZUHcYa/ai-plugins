# Changelog

All notable changes to music-production will be documented in this file.

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
