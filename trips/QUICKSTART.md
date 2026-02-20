# trips Quick-Start Guide

Get started with trips in 5 minutes.

---

## Installation

### Option A: Claude Code Plugin (Recommended)

**Step 1:** Add the marketplace
```
/plugin marketplace add ZUHcYa/ai-plugins
```

**Step 2:** Install the plugin
```
/plugin install trips@ai-plugins
```

**Step 3:** Run setup
```
/trips:start
```

### Option B: Manual Setup

1. Create `.trips/config.json` with `dataDir` and `company` fields
2. Create the folder structure manually: `<dataDir>/<YYYY>/<MM - YYYY>/<Name>/`
3. Create trip files using the naming convention: `YYYY-MM-DD_destination.md`

### Updates

To get the latest version, pull the latest changes from the repository or re-install the plugin.

---

## 1. Setup

```
/trips:start
```

On first run, trips asks for your data directory and company name:

```
User: /trips:start
Claude: Welcome to trips!

        Where should trip files be stored?
        (e.g. "O/Backoffice/Auslagen" or "./trips-data")

User: O/5 Support Processes/53 Backoffice/Auslagen

Claude: What's your company name?

User: Acme GmbH

Claude: Created: .trips/config.json

        Ready! Run /trips:new to record your first trip.
```

---

## 2. Record a Trip

```
/trips:new
```

Records a new business trip with all required fields:

```
User: /trips:new
Claude: Trip description (name)?

User: Client meeting Berlin

Claude: Date(s)? (e.g. 2026-02-03 or 2026-02-03 to 2026-02-05)

User: 2026-02-03

Claude: Destination?

User: Berlin

Claude: Business purpose?

User: Q1 planning meeting with [Kunde]

Claude: Distance driven (km)? Leave empty if none.

User: 320

Claude: Created: O/5 Support Processes/53 Backoffice/Auslagen/2026/02 - 2026/Friedrich Wagner/2026-02-03_berlin.md

        Trip recorded:
        - 1 travel day
        - Distance: 320 km → Reimbursement: €96.00
```

---

## 3. List Trips

```
/trips:list
```

Lists all trips for a given month or date range:

```
User: /trips:list
Claude: February 2026 — Friedrich Wagner

        2026-02-03  Berlin          Client meeting          320 km  €96.00
        2026-02-10  Munich          Partner workshop        410 km  €123.00
        2026-02-17  Hamburg         Conference              290 km  €87.00

        Total: 3 trips | 1,020 km | €306.00
```

---

## 4. Edit a Trip

```
/trips:edit
```

Modify an existing trip record. Useful if you forgot to enter the distance or made an error.

---

## 5. Generate Monthly Report

```
/trips:report
```

Generates a monthly summary ready for your tax return:

```
User: /trips:report
Claude: Generated: _report-2026-02.md

        February 2026 | Friedrich Wagner | Acme GmbH
        -----------------------------------------------
        3 trips | 1,020 km | €306.00 total reimbursement

        PDF export: _report-2026-02.pdf (via markdown2pdf-mcp)
```

The report is saved alongside your trip files. PDF export requires Node.js 18+ and the `markdown2pdf-mcp` MCP server.

---

## The Complete Workflow

```
/trips:start → /trips:new → /trips:list → /trips:report
     |              |            |             |
   Setup        Record trip   Review month  Generate report
```

---

## Helpful Skills

| Skill | When to use |
|-------|-------------|
| `/trips:start` | Setup (first run) or quick status check |
| `/trips:new` | Record a new business trip |
| `/trips:edit` | Correct an existing trip |
| `/trips:list` | Review trips for a month |
| `/trips:report` | Generate monthly Finanzamt report |

---

## Manual Workflow (No Skills)

trips works without Claude Code skills too:

1. Create the folder: `<dataDir>/2026/02 - 2026/Your Name/`
2. Create file: `2026-02-03_berlin.md`
3. Add YAML frontmatter (see CLAUDE.md for required fields)
4. Calculate reimbursement: `distanceKm × 0.30`

All files are standard Markdown — use any editor (Obsidian, VS Code, Notion, etc.).
