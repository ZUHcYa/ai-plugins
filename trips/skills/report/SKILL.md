---
name: "report"
description: "Generate tax-relevant monthly summary for Finanzamt"
---

# /trips:report

## Purpose

Generates a comprehensive monthly Dienstreisen report suitable for the German Finanzamt.
Displays the report in conversation and optionally saves it as a markdown file.

## When to Use

- Monthly travel expense reporting
- Steuerberater needs a monthly trip summary
- Consolidated monthly export for Auslagen

## Workflow

### Monthly Report (Default)

```
User: /trips:report
Claude: Report fuer welchen Monat? [2026-02]

User: [Enter]
Claude: Dienstreisen-Report Februar 2026
        Friedrich Wagner | ACME GmbH
        ======================================

        Erstattung
        ----------
        580 km x 0,30 EUR/km = **174,00 EUR**

        Alle Reisen
        -----------
        | # | Datum | Reise | km | EUR |
        |---|-------|-------|----|-----|
        | 1 | 03.02. | Berlin – Praesentation Q1 | 580 | 174,00 |
        | 2 | 10.-12.02. | Muenchen – Team Workshop | - | - |

        Report speichern als ...\2026\02 - 2026\Friedrich Wagner\_report-2026-02.md? [y/N]

User: y
Claude: Gespeichert: ...\2026\02 - 2026\Friedrich Wagner\_report-2026-02.md
        PDF erzeugt: ...\2026\02 - 2026\Friedrich Wagner\_report-2026-02.pdf
```

---

## What the Skill Does

1. Read `.trips/config.json`. If missing: error with hint to run `/trips:start`
2. Resolve dataDir from workspace root (cwd)
3. Determine scope:
   - No argument: ask for month (default: current year-month, e.g. `2026-02`)
   - `YYYY-MM`: monthly report for that month
4. **Pfad konstruieren** - diese Variable fuer alle weiteren Schritte verwenden:
   `employeeDir = <dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`
   Beispiel: `O/5 Support Processes/53 Backoffice/Auslagen/2026/02 - 2026/Friedrich Wagner/`
   **WICHTIG: NICHT den Monatsordner verwenden. Der Employee-Unterordner ist PFLICHT.**
5. Read all trip files from `{employeeDir}` (skip `_report-*.md`), parse YAML frontmatter
6. Calculate totals: km, reimbursementEur
7. Create chronological list of all trips (numbered)
8. Display report with employee header (`<Name> | <Company>`) and Erstattung block
9. Optionally save to `{employeeDir}/_report-YYYY-MM.md`
    - **Wenn Write-Tool fehlschlaegt:** Explizit melden: "Datei konnte nicht gespeichert werden (Dateisystem nicht verfuegbar). Der Report oben ist vollstaendig - bitte manuell kopieren." PDF-Schritt ueberspringen (ohne Dateisystem sinnlos). Report-Anzeige gilt als Erfolg.
10. **PFLICHT: PDF generieren.** Unmittelbar nach erfolgreichem Speichern ausfuehren - niemals stillschweigend ueberspringen.
    - MCP-Tool `create_pdf_from_markdown` aufrufen mit:
      - `markdown`: vollstaendiger Markdown-Inhalt (ohne YAML-Frontmatter)
      - `paperFormat`: "a4"
      - `paperOrientation`: "landscape"
      - `paperBorder`: "2cm"
      - `showPageNumbers`: true
      - `outputFilename`: `_report-YYYY-MM.pdf` (NUR Dateiname, KEIN Pfad! Das MCP-Tool konkateniert intern das Output-Verzeichnis.)
    - Nach erfolgreicher PDF-Erzeugung: Datei vom MCP-Output-Verzeichnis (Standard: HOME-Verzeichnis) nach `{employeeDir}/_report-YYYY-MM.pdf` verschieben
    - **Wenn MCP-Tool nicht verfuegbar:** Explizit melden: "PDF uebersprungen: MCP nicht verfuegbar. Markdown-Report ist vollstaendig."
    - **Wenn MCP-Tool fehlschlaegt:** Fehler anzeigen, aber Report-Erstellung als erfolgreich werten.

---

## Report File Format

When saved, the report file uses YAML frontmatter + same markdown content:

```yaml
---
type: trips-report
month: "2026-02"
generated: "2026-02-28"
employee:
  firstName: "Friedrich"
  lastName: "Wagner"
  company: "ACME GmbH"
totalTrips: 2
totalKm: 580
totalReimbursementEur: 174.00
---

# Dienstreisen-Report Februar 2026

Friedrich Wagner | ACME GmbH

[... same markdown content as displayed ...]
```

### File Naming

| Scope | Markdown | PDF | Location |
|-------|----------|-----|----------|
| Monthly | `_report-YYYY-MM.md` | `_report-YYYY-MM.pdf` | `{employeeDir}` (= `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`) |

The `_` prefix sorts generated files before trip files and signals they are not data sources.
PDF is generated automatically via MCP when the markdown report is saved.
**Niemals direkt im Monatsordner speichern - immer im Employee-Unterordner.**

---

## Erstattung Calculation in Reports

**Per trip:** Read `reimbursementEur` from YAML (already calculated by :new or :edit).

**Total:** Sum all `reimbursementEur` values. Show calculation:
`4.860 km x 0,30 EUR/km = 1.458,00 EUR`

If total km and total EUR don't match the formula (e.g. due to manual edits),
show both: total from YAML and recalculated value, flag the discrepancy.

---

## Month Names (German)

| Month | Name |
|-------|------|
| 1 | Januar |
| 2 | Februar |
| 3 | Maerz |
| 4 | April |
| 5 | Mai |
| 6 | Juni |
| 7 | Juli |
| 8 | August |
| 9 | September |
| 10 | Oktober |
| 11 | November |
| 12 | Dezember |

Use ASCII-safe German (Maerz not Maerz) consistent with the codebase.

---

## Edge Cases

| Scenario | Behavior |
|----------|----------|
| No trips for month | "Keine Reisen fuer YYYY-MM erfasst." |
| No distanceKm on any trip | Omit km and EUR columns entirely |
| Mixed trips (some with km, some without) | Show `-` for trips without km |
| Regenerating existing report | Overwrite previous `_report-*.md` and `_report-*.pdf` |
| MCP tool not available | Inform user: "PDF uebersprungen: MCP nicht verfuegbar." Continue with .md |
| Write-Tool fehlschlaegt | Fehler melden, Report in Konversation ist vollstaendig. PDF ueberspringen. |

---

## PDF Generation

Reports are automatically converted to PDF when saved, using the MCP tool `create_pdf_from_markdown`
(provided by the bundled `markdown2pdf` MCP server).

**Parameters used:**
- `paperFormat`: "a4"
- `paperOrientation`: "landscape"
- `paperBorder`: "2cm"
- `showPageNumbers`: true
- `outputFilename`: NUR der Dateiname (z.B. `_report-2026-02.pdf`), KEIN voller Pfad

**WICHTIG:** `outputFilename` akzeptiert nur Dateinamen. Das MCP-Tool konkateniert intern
sein Output-Verzeichnis (`M2P_OUTPUT_DIR`, default: HOME) mit dem Dateinamen.
Nach Erzeugung die PDF vom Output-Verzeichnis nach `{employeeDir}` verschieben.

**Graceful degradation:** If the MCP tool is not available (e.g. user runs without Claude Code,
or MCP server is not configured), inform the user ("PDF uebersprungen: MCP nicht verfuegbar.
Markdown-Report ist vollstaendig.") and continue. The markdown report is the primary output
and fully self-contained.

**Degradation-Hierarchie:**
1. Vollstaendig: Report anzeigen + .md speichern + .pdf erzeugen
2. Ohne MCP: Report anzeigen + .md speichern (PDF uebersprungen)
3. Ohne Dateisystem: Report anzeigen (Speichern + PDF uebersprungen)

In allen drei Faellen gilt der Report als erfolgreich erstellt.

---

## Notes

- Default scope is current month (user presses Enter to accept)
- Report is always displayed in conversation first
- Save to file is optional (user must confirm)
- Report file uses `_` prefix to distinguish from trip data files
