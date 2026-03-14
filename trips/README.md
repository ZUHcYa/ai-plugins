# trips - Business Travel Tracker

Track Dienstreisen for your German tax return (Steuererklaerung).

## What It Does

**trips** manages your business travel records for the Finanzamt:

- **Recording** new trips with date, destination, purpose, and distance
- **Listing** trips by month or date range
- **Editing** existing trip records
- **Generating** a monthly Finanzamt-ready summary report (with optional PDF export)

Kilometerpauschale (§ 9 Abs. 1 Satz 3 Nr. 4a EStG) is calculated automatically.

## Quick Start

```
/trips:start          Setup on first run, status overview afterwards
/trips:new            Record a new business trip
/trips:list           List trips (by month or date range)
/trips:edit           Edit an existing trip record
/trips:report         Generate monthly Finanzamt summary
```

## Skills

| Skill | Purpose |
|-------|---------|
| `/trips:start` | Setup on first run, overview afterwards |
| `/trips:new` | Record a new business trip |
| `/trips:list` | List and filter recorded trips |
| `/trips:edit` | Edit an existing trip record |
| `/trips:report` | Generate a tax-relevant monthly summary |

## State

Config lives in `.trips/config.json` (`dataDir` and `company`).
Trip records are stored as individual Markdown files with YAML frontmatter.

```
<dataDir>/
  2026/
    02 - 2026/
      Max Mustermann/
        2026-02-03_berlin.md
        2026-02-15_muenchen.md
        _report-2026-02.md
        _report-2026-02.pdf
```

## Automatic Hooks

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows the last recorded trip and a reminder to use `/trips:new` |

The hook is advisory only — it informs, never blocks.

## PDF Export (MCP Server)

The plugin ships with an optional MCP server (`markdown2pdf-mcp`) that generates PDFs
from monthly reports.

- **Server:** `markdown2pdf-mcp` (Node.js + Puppeteer, on-demand via npx)
- **Tool:** `create_pdf_from_markdown` — automatically called by `/trips:report`
- **`outputFilename`** accepts ONLY filenames (e.g. `_report-2026-02.pdf`), not paths. Saves to `M2P_OUTPUT_DIR` (default: HOME). Move the PDF to the target folder afterwards.
- **Prerequisites:** Node.js 18+ and npm (Puppeteer downloads ~170 MB Chromium on first start)
- Without Node.js: PDF export is skipped, all other features work normally

MCP is an automation layer, not a core component. The Markdown report remains fully usable without it.

## Manual Workflow (No Skills)

trips works without Claude Code:

1. Create the folder: `<dataDir>/2026/02 - 2026/Your Name/`
2. Create file: `2026-02-03_berlin.md`
3. Add YAML frontmatter (see [STRUCTURE.md](STRUCTURE.md) for required fields)
4. Calculate reimbursement: `distanceKm x 0.30`

All files are standard Markdown — use any editor.

## Documentation

- [STRUCTURE.md](STRUCTURE.md) - Folder structure, file format, and field reference
- [CHANGELOG.md](CHANGELOG.md) - Version history
