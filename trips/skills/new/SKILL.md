---
name: "new"
description: "Record a new business trip"
---

# /trips:new

## Purpose

Records a new Dienstreise by creating a trip file in the appropriate employee folder.
Collects trip details interactively and generates a human-readable markdown document.

## When to Use

- Recording a business trip (past or planned)
- Adding a trip you forgot to log

## Workflow

```
User: /trips:new
Claude: Reise-Name (kurze Beschreibung)?

User: Kundentermin Berlin
Claude: Startdatum (YYYY-MM-DD)?

User: 2026-02-03
Claude: Enddatum (YYYY-MM-DD)? ["-" = gleicher Tag]

User: -
Claude: Ziel (Ort)?

User: Berlin
Claude: Zweck der Reise?

User: Kundenpraesentation Q1
Claude: Abfahrt (HH:MM)? [optional, "-" = skip]

User: 07:30
Claude: Rueckkehr (HH:MM)? [optional, "-" = skip]

User: 19:00
Claude: Entfernung in km? [optional, "-" = skip]

User: 580
Claude: Reise erfasst!

        Datei: ...\Auslagen\2026\02 - 2026\Friedrich Wagner\2026-02-03_berlin.md
        Reise: Kundentermin Berlin
        Datum: 03.02.2026
        Ziel:  Berlin (580 km)
        Erstattung: 580 km x 0,30 EUR/km = 174,00 EUR
```

---

## What the Skill Does

1. Read `.trips/config.json`. If missing: error with hint to run `/trips:start`
2. Resolve dataDir from workspace root (cwd)
3. Interactively collect trip details:
   - name (required)
   - startDate (required, YYYY-MM-DD)
   - endDate (required, default = startDate if user presses Enter)
   - destination (required)
   - purpose (required)
   - startTime (optional, HH:MM)
   - endTime (optional, HH:MM)
   - distanceKm (optional, positive number)
4. Calculate reimbursementEur if distanceKm provided: `distanceKm x 0,30`
5. Generate filename: `YYYY-MM-DD_<destination-kebab>.md`
6. Create employee folder if needed: `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`
7. Write trip file: YAML frontmatter + body table + empty Notizen section
8. Display confirmation with summary

---

## Trip File Template

```markdown
---
type: trip
name: "Kundentermin Berlin"
startDate: "2026-02-03"
endDate: "2026-02-03"
destination: "Berlin"
purpose: "Kundenpraesentation Q1"
startTime: "07:30"
endTime: "19:00"
distanceKm: 580
reimbursementEur: 174.00
created: "2026-02-03"
---

# Kundentermin Berlin

| Feld | Wert |
|------|------|
| Datum | 03.02.2026 |
| Ziel | Berlin |
| Zweck | Kundenpraesentation Q1 |
| Abfahrt | 07:30 |
| Rueckkehr | 19:00 |
| Strecke | 580 km |
| Erstattung | 580 km x 0,30 EUR/km = **174,00 EUR** |

## Notizen

```

### Template Rules

- **YAML contains all structured data** - this is the source of truth for skills
- **Body table is the human-readable view** - shows the same data formatted for readers
- **Notizen section is never overwritten** by skills (safe space for user content)
- **Optional fields** (startTime, endTime, distanceKm, reimbursementEur): omit from YAML if not provided, omit row from table
- **reimbursementEur** is only set when distanceKm is provided

### Multi-Day Trip Example

```markdown
---
type: trip
name: "Workshop Muenchen"
startDate: "2026-02-10"
endDate: "2026-02-12"
destination: "Muenchen"
purpose: "Team Workshop"
created: "2026-02-10"
---

# Workshop Muenchen

| Feld | Wert |
|------|------|
| Datum | 10.02. - 12.02.2026 (3 Reisetage) |
| Ziel | Muenchen |
| Zweck | Team Workshop |

## Notizen

```

---

## Filename Convention

`YYYY-MM-DD_<destination-kebab-case>.md`

| Part | Format | Example |
|------|--------|---------|
| Date | `YYYY-MM-DD` from startDate | `2026-02-03` |
| Separator | `_` | `_` |
| Destination | lowercase, kebab-case | `berlin`, `frankfurt-am-main` |
| Extension | `.md` | `.md` |

**Rules:**
- Lowercase only
- No umlauts: ue->ue, ae->ae, oe->oe, ss->ss (muenchen, not münchen)
- Spaces become hyphens
- No special characters

**Duplicate handling:** If a file with the same name already exists (two trips to the same
city on the same day), append a counter: `2026-02-03_berlin-2.md`

---

## Datum Column Format

| Scenario | Format | Example |
|----------|--------|---------|
| Same day | `DD.MM.YYYY` | `03.02.2026` |
| Multi-day, same month | `DD.MM. - DD.MM.YYYY (N Reisetage)` | `10.02. - 12.02.2026 (3 Reisetage)` |
| Multi-day, cross-month | `DD.MM. - DD.MM.YYYY (N Reisetage)` | `28.02. - 02.03.2026 (3 Reisetage)` |

---

## Erstattung Calculation

**Formula:** distanceKm x 0,30 EUR/km
**Legal basis:** Kilometerpauschale gemaess § 9 Abs. 1 Satz 3 Nr. 4a EStG

Always show the full calculation in the body table:
`580 km x 0,30 EUR/km = **174,00 EUR**`

Store result in YAML as `reimbursementEur: 174.00` (2 decimal places).

---

## Cross-Month Trips

If a trip spans months (e.g. startDate 2026-02-28, endDate 2026-03-02), the trip file
is stored in the employee folder of the **startDate** month
(`<dataDir>/<YYYY>/<02 - YYYY>/<FirstName LastName>/`). One trip, one file, one location.

---

## Input Validation

| Field | Validation |
|-------|-----------|
| name | Non-empty string |
| startDate | Valid date, YYYY-MM-DD format |
| endDate | Valid date, >= startDate |
| destination | Non-empty string |
| purpose | Non-empty string |
| startTime | HH:MM format (00:00-23:59) |
| endTime | HH:MM format (00:00-23:59) |
| distanceKm | Positive number |

---

## Notes

- Requires config (run `/trips:start` first)
- YAML frontmatter is the single source of truth
- Notizen section is never touched by skills - safe for user content
- endDate defaults to startDate for single-day trips
- Optional fields are omitted entirely from YAML and table when not provided
- reimbursementEur is calculated, not user-entered
