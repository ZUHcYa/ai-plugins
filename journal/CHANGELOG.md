# Changelog

All notable changes to journal will be documented in this file.

---

## [0.2.1] - 2026-02-27

### Added

- `/journal:write` skill: interactive guided entry mode — asks for topic, content, and tasks step by step
- Restores the conversational workflow alongside the `/journal` quick command

---

## [0.2.0] - 2026-02-27

### Added

- `/journal` command (`commands/journal.md`): direct quick-entry without interaction — write to today's file instantly
- Supports `#tag content` syntax for topic tagging in one line

### Removed

- `/journal` skill (interactive mode) — replaced by the command above
- Two-step interaction (ask for topic, ask for content) is no longer part of `/journal`

---

## [0.1.0] - 2026-02-21

### Added

- Initial plugin release: personal daily journal with timestamps
- 2 skills: `/journal:start`, `/journal`
- Smart entry point: auto-detects first run vs. returning user
- Journal entry creation: interactive mode (asks for topic + content) and one-shot mode (direct append)
- Day file format: one Markdown file per day (YYYY-MM-DD.md) with YAML frontmatter
- Timestamped entries with optional topic tags
- Task management: inline Markdown checkboxes or external system reference
- SessionStart hook: shows today's entry count or last entry date
- Overview with statistics: entry counts, streak tracking, top tags
- Configurable data directory and filename pattern
- Tool-agnostic design: all files are plain Markdown — works in any editor
