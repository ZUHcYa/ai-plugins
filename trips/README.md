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

Config lives in `.trips/config.json`. Trip records are stored as individual Markdown files
with YAML frontmatter - readable in any editor, no Claude Code required.

Each trip is a file in your configured data directory:

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

File naming: `YYYY-MM-DD_<destination-kebab-case>.md`

## Automatic Hooks

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows the last recorded trip and a reminder to use `/trips:new` |

The hook is advisory only — it informs, never blocks.

## PDF Export

The plugin ships with an optional MCP server (`markdown2pdf-mcp`) that generates PDFs
from monthly reports. Requires Node.js 18+. Without it, the Markdown report works as-is.

## Documentation

- [QUICKSTART.md](QUICKSTART.md) - 5-minute getting started guide
- [STRUCTURE.md](STRUCTURE.md) - Folder structure and file format reference
- [CHANGELOG.md](CHANGELOG.md) - Version history
