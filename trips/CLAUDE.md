# trips: Plugin-Specific Rules

## Vision

Business travel tracking (Dienstreisen) for the German tax return (Steuererklaerung). Record trips, list
them, and generate Finanzamt-ready reports. No lifecycle - pure data entry and reporting.

One Markdown file per trip is the central format. YAML frontmatter is the single
source of truth; the Markdown body provides a human-readable view plus notes.

### Strict Rules

- **No infrastructure features** without explicit review
- **No scope creep** - trips is a recording and reporting tool, not a full accounting system
- **No external dependencies** beyond the optional MCP server for PDF export

### Versioning

`CHANGELOG.md` is the single source of truth for the version. `plugin.json` is synced automatically
via git pre-commit hook — never edit it manually.

| Change type | Version bump |
|-------------|-------------|
| New feature, Breaking change in Skills/Commands | Minor (`0.x.0`) |
| Bugfix, docs correction | Patch (`0.x.1`) |

Always add a `## [x.y.z] - YYYY-MM-DD` entry to `CHANGELOG.md` before committing.

### Warning-Signal Check

**Before each task, ask:**
1. Does this help users **record or report business trips**?
2. Does this stay within the established data format and folder structure?

**When in doubt:** Ask the developer immediately!

---

## Calculations

**Kilometerpauschale:** distanceKm x 0.30 EUR/km (§ 9 Abs. 1 Satz 3 Nr. 4a EStG).
Always show the calculation in the body: `580 km x 0.30 EUR/km = 174.00 EUR`.

**Travel days (Reisetage):** Calendar days from startDate to endDate inclusive.
Same day = 1 travel day.

---

## Path Handling

- **Bash commands:** Always quote paths (`"path with spaces"`)
- **Write/Read/Edit/Glob:** Handle spaces correctly, no quoting needed
- Do NOT verify via Bash/ls after Write calls (can produce false "not found" errors)

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/trips:` prefix | `/trips:start`, `/trips:new` |
| Skill files | `skills/<skill>/SKILL.md` | `skills/start/SKILL.md` |
| Configuration | `.trips/` folder | `.trips/config.json` |
| Trip files | `YYYY-MM-DD_destination.md` | `2026-02-03_berlin.md` |

---

## Tool-Agnostic: Manual Workflow Always Possible

All files are pure Markdown with standard frontmatter. Skills are helpers, not prerequisites.
Users can create and edit files manually. No proprietary formats, no tool-specific dependencies.

Data structure, file formats, and path construction rules are documented in [STRUCTURE.md](STRUCTURE.md).
