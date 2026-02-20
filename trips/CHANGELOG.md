# Changelog

All notable changes to trips will be documented in this file.

---

## [0.4.0] - 2026-02-20

### Added

- Initial plugin release: German tax-compliant business travel tracking
- 5 skills: `/trips:start`, `/trips:new`, `/trips:edit`, `/trips:list`, `/trips:report`
- YAML frontmatter as single source of truth for trip data
- Automatic Kilometerpauschale calculation: distance × €0.30 (§ 9 Abs. 1 Satz 3 Nr. 4a EStG)
- Travel day calculation: inclusive calendar days (same-day trip = 1 day)
- Dynamic path construction: `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`
- Monthly report generation: Markdown summary + optional PDF via MCP server
- Optional MCP integration: `markdown2pdf-mcp@2.1.3` for PDF export (requires Node.js 18+)
- Graceful degradation: Markdown reports work without Node.js, PDF is optional
- Tool-agnostic design: all trip files are plain Markdown — works in any editor

### Architecture

- ADR-007: Plugin = instruction layer only, no infrastructure
- ADR-008: MCP server is optional enhancement, not core dependency
- Config stored in `.trips/config.json` (dataDir, company)
