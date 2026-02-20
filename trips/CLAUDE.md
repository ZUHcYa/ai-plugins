# trips: Plugin-Specific Rules

## Vision

Business travel tracking (Dienstreisen) for the German tax return (Steuererklaerung). Record trips, list
them, and generate Finanzamt-ready reports. No lifecycle - pure data entry and reporting.

One Markdown file per trip is the central format. YAML frontmatter is the single
source of truth; the Markdown body provides a human-readable view plus notes.

### Strict Rules

- **No infrastructure features** without explicit review (no automated deployments, no background services)
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

## Data Structure

### Configuration

One file: `.trips/config.json` (shared/team data only)

```json
{
  "dataDir": "O/5 Support Processes/53 Backoffice/Auslagen",
  "company": "Acme GmbH"
}
```

**Setup:** `/trips:start` asks for `dataDir` and `company`. A project CLAUDE.md can provide
`## Trips Defaults` with both values - in that case no setup is needed.

### Employee Name

Comes from the Claude context (User Preferences / `~/.claude/CLAUDE.md`), not from config.
If the name is not available from context: ask the user.

### Data Directory (user-configured)

```
<dataDir>/
  2026/
    01 - 2026/
      Friedrich Wagner/
        2026-01-15_berlin.md
        2026-01-20_muenchen.md
        _report-2026-01.md
        _report-2026-01.pdf
    02 - 2026/
      Friedrich Wagner/
        2026-02-03_berlin.md
```

### Path Construction

All trip files and reports live in the employee folder:

```
<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/
```

- `<YYYY>`: 4-digit year
- `<MM - YYYY>`: Two-digit month + ` - ` + year (e.g. `01 - 2026`). Leading zero.
- `<FirstName LastName>`: From Claude context (User Preferences), original spelling.
- Create folders if needed (mkdir -p equivalent).

`dataDir` is relative to the workspace root (cwd). Use forward slashes.

### Path Handling

- **Bash commands:** Always quote paths (`"path with spaces"`)
- **Write/Read/Edit/Glob:** Handle spaces correctly, no quoting needed
- Do NOT verify via Bash/ls after Write calls (can produce false "not found" errors)

### Trip File Format

YAML frontmatter = single source of truth. Body = table + notes.

Fields per trip:
- name (string, required) - Short trip description
- startDate (YYYY-MM-DD, required) - Start date
- endDate (YYYY-MM-DD, required) - End date
- destination (string, required) - Destination
- purpose (string, required) - Business purpose
- startTime (HH:MM, optional) - Departure time
- endTime (HH:MM, optional) - Return time
- distanceKm (number, optional) - Distance driven in km
- reimbursementEur (number, optional) - Km reimbursement (distanceKm x 0.30)
- created (YYYY-MM-DD, auto) - Creation date

### Calculations

**Kilometerpauschale:** distanceKm x 0.30 EUR/km (§ 9 Abs. 1 Satz 3 Nr. 4a EStG).
Always show the calculation in the body: `580 km x 0.30 EUR/km = 174.00 EUR`.
Store the result as `reimbursementEur` in YAML.

**Travel days (Reisetage):** Calendar days from startDate to endDate inclusive.
Same day = 1 travel day. Feb 10 to Feb 12 = 3 travel days.

### File Naming Rule

`YYYY-MM-DD_<destination-kebab-case>.md`

Examples:
- `2026-02-03_berlin.md`
- `2026-01-20_muenchen.md`
- `2026-03-15_frankfurt-am-main.md`

Rules: Lowercase, no umlauts (use ue instead of ü), hyphens instead of spaces.

### Report Files

`.md` is the source of truth. `.pdf` is the generated export format (via MCP, see below).

| Scope | Markdown | PDF | Location |
|-------|----------|-----|----------|
| Monthly report | `_report-YYYY-MM.md` | `_report-YYYY-MM.pdf` | `<dataDir>/<YYYY>/<MM - YYYY>/<Name>/` |

---

## MCP Server: markdown2pdf

The plugin ships with an MCP server (`trips/.mcp.json`) that starts automatically
when the plugin is activated. It converts Markdown reports to PDF.

**Server:** `markdown2pdf-mcp` (Node.js + Puppeteer, on-demand via npx)
**Tool:** `create_pdf_from_markdown`
**Usage:** Automatically called by `/trips:report` after saving the .md file.

MCP is an automation layer - not a core component (see ADR-007).
Without MCP, the Markdown report remains fully usable.

**IMPORTANT:** `outputFilename` accepts ONLY filenames (e.g. `_report-2026-02.pdf`), not paths.
The tool saves to `M2P_OUTPUT_DIR` (default: HOME). Move the PDF to the target folder afterwards.

**Prerequisites for PDF export:**
- Node.js 18+ and npm (for npx)
- Puppeteer downloads ~170 MB Chromium on first start
- Without Node.js: PDF export is skipped, all other features work normally

**Version maintenance:** Version is pinned in `.mcp.json`. Check when needed:
`npm view markdown2pdf-mcp version` — update `.mcp.json` when a new version is available.

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/trips:` prefix | `/trips:start`, `/trips:new` |
| Skill files | `skills/<skill>/SKILL.md` | `skills/start/SKILL.md` |
| Configuration | `.trips/` folder | `.trips/config.json` |
| Trip files | `YYYY-MM-DD_destination.md` | `2026-02-03_berlin.md` |

### Conflict Avoidance

- State files in `.trips/` (hidden folder, gitignored)
- Clear separation via `/trips:` namespace
- Data directory freely configurable by the user

---

## Markdown Generation

**IMPORTANT:** Users and tax advisors (Steuerberater) read trip files in Markdown viewers
(Obsidian, VS Code) AND Claude Code processes them.

**Audience:**
- **Primary:** User / Steuerberater (humans with Markdown viewer)
- **Secondary:** Claude Code / AI (must be able to parse YAML frontmatter)

All generated files must work well in both contexts.

---

## Emoji Usage

**Rule:** Emojis only where they add real value - not as decoration.

### Allowed
- Status indicators where text alone would be unclear (e.g. in compact tables)

### Forbidden
- Decorative emojis without function
- Multiple emojis per line/heading
- Emojis in body text
- "AI-slop" style

---

## Tool-Agnostic: Manual Workflow Always Possible

**Core principle:** The user must always be able to record Dienstreisen **manually without
skills**. The chosen tool (Obsidian, VS Code, etc.) must not matter.

**Rules:**
- Do not build features that only work with specific tools
- All files are pure Markdown with standard frontmatter
- Skills are helpers, not prerequisites
- Users can create and edit files manually
- Folder structure and file format are self-explanatory

**Forbidden:**
- Obsidian-specific plugins or Dataview queries as core functionality
- Proprietary file formats
- Features that only work with Claude Code
- Dependencies on external APIs or services
