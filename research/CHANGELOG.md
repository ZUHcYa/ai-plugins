# Changelog

All notable changes to research will be documented in this file.

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
