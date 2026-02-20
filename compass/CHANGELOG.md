# Changelog

All notable changes to compass will be documented in this file.

---

## [0.1.0] - 2026-02-20

### Added

- Initial release: session goal alignment, vision tracking, automatic drift detection
- 5 skills: `/compass:init`, `/compass:focus`, `/compass:check`, `/compass:challenge`, `/compass:retro`
- 3 hooks: SessionStart (vision + goal status), PreToolUse Write/Bash (alignment check), Stop (drift detection)
- `.compass/` state directory: `VISION.md` (project vision), `rules.md` (custom rules), `session.md` (session goal)
- Soft/advisory hook design: warns, never blocks. Always returns `approve`
- Built-in heuristics: YAGNI, Scope Creep, Over-Engineering, Gold Plating, Premature Optimization, NIH Syndrome
