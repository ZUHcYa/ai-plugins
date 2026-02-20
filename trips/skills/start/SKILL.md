---
name: "start"
description: "Smart entry point - setup on first run, overview afterwards"
---

# /trips:start

## Purpose

Single entry point for trips. Automatically detects context:
- **First run:** Sets up config and data directory
- **After setup:** Shows overview of all recorded trips

## When to Use

- Starting with trips for the first time
- Beginning a session - get a quick overview
- After a break - see the current state
- Quick status check across years

---

## First Run (No Config)

When no `.trips/config.json` exists, setup runs automatically.

### With Project Defaults

Check the project's CLAUDE.md for a `## Trips Defaults` section. If found, use the
pre-configured `dataDir` and `company` values - no interactive setup needed:

```
User: /trips:start
Claude: Welcome to trips!
        [dataDir und Firma aus Projekt-Defaults uebernommen]

        Created: .trips/config.json (dataDir: O/5 Support.../Auslagen, Firma: Acme GmbH)
        Created: O/5 Support.../Auslagen/02 - 2026/Max Mustermann/

        Ready! Run /trips:new to record your first trip.
```

### Without Project Defaults

If no `## Trips Defaults` in CLAUDE.md, ask for shared values interactively:

```
User: /trips:start
Claude: Welcome to trips!

        Firma?
User: ACME GmbH

Claude: Wo sollen die Reisedaten gespeichert werden?
        (Relativer Pfad vom Workspace, z.B. "O/5 Support Processes/53 Backoffice/Auslagen")

User: O/5 Support Processes/53 Backoffice/Auslagen
Claude: Setting up...

        Created: .trips/config.json
        Created: O/5 Support.../Auslagen/2026/02 - 2026/Friedrich Wagner/

        Ready! Run /trips:new to record your first trip.
```

Employee name (here: "Friedrich Wagner") comes from Claude context (user preferences),
not from config. If unknown, Claude asks.

---

## After Setup (Config Exists)

When config exists, read all year/month folders and trip files, display overview:

```
User: /trips:start
Claude: Dienstreisen Status
        ======================================

        2026 (12 Reisen, 28 Reisetage, 4.860 km, 1.458,00 EUR)
        ----------------------------------------------------------
        Jan:  3 Reisen,  8 Tage, 1.200 km,   360,00 EUR
        Feb:  2 Reisen,  3 Tage,   580 km,   174,00 EUR
        ...

        2025 (45 Reisen, 98 Reisetage, 18.400 km, 5.520,00 EUR)
        ----------------------------------------------------------
        [summary per month]

        Gesamt: 57 Reisen, 126 Reisetage, 23.260 km, 6.978,00 EUR

        Aktionen
        --------
        1. Letzter Eintrag: 12.02.2026 (Workshop Muenchen)
        2. /trips:new    -> neue Reise erfassen
        3. /trips:list   -> Reisen anzeigen
        4. /trips:report -> Monats-Report
```

### Empty Data Directory

```
User: /trips:start
Claude: Dienstreisen Status
        ======================================

        Noch keine Reisen erfasst.

        Get Started
        -----------
        Run /trips:new to record your first trip!
```

---

## What the Skill Does

### First Run (Setup)

1. Check if `.trips/config.json` exists
2. If NOT:
   a. Search CLAUDE.md for `## Trips Defaults` section
      - If found: extract `dataDir` and `company` (no interactive setup needed)
      - If not found: ask for `dataDir` and `company` interactively
   b. Write `.trips/config.json` with `dataDir` and `company`
   c. Resolve dataDir from workspace root (cwd)
   d. Get employee name from Claude context (user preferences). If unknown: ask.
   e. Create employee folder for current month: `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`

### After Setup (Overview)

1. Read `.trips/config.json`
2. Resolve dataDir from workspace root (cwd)
3. Verify dataDir exists
4. Scan all year folders in dataDir
5. For each year: scan month folders, then employee name subfolder, read all trip `.md` files (excluding `_report-*.md`), parse YAML frontmatter
6. Calculate per year: total trips, total Reisetage, total km, total reimbursementEur
7. Calculate per month: same metrics
8. Display summary grouped by year (newest first)
9. Show last recorded trip and suggested actions

### Reisetage Calculation

Calendar days from startDate to endDate inclusive.
- Same day: 1 Reisetag
- 2026-02-10 to 2026-02-12: 3 Reisetage

### Km-Erstattung

Sum of `reimbursementEur` from all trip YAML frontmatter.

---

## Notes

- Single command to remember: just `/trips:start`
- Context-aware: detects first run vs. returning user
- Plugin creates data directory and folders as needed
- Actionable suggestions help users know what to do next
- Years shown newest-first for quick orientation
