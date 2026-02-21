---
name: "edit"
description: "Edit an existing trip record"
---

# /trips:edit

## Purpose

Corrects or updates a previously recorded Dienstreise. Modifies YAML frontmatter,
regenerates the body table, and preserves user-written Notizen.

## When to Use

- Fixing a typo in destination or purpose
- Correcting dates or times
- Adding missing km data after the fact
- Updating any trip field

## Workflow

### Without Argument (Browse Current Month)

```
User: /trips:edit
Claude: Reisen im Februar 2026:

        1. 03.02. Kundentermin Berlin (berlin)
        2. 10.02. Workshop Muenchen (muenchen)

        Welche Reise bearbeiten? (Nummer)

User: 1
Claude: Kundentermin Berlin (2026-02-03_berlin.md)

        1. name:        Kundentermin Berlin
        2. startDate:   2026-02-03
        3. endDate:     2026-02-03
        4. destination: Berlin
        5. purpose:     Kundenpraesentation Q1
        6. startTime:   07:30
        7. endTime:     19:00
        8. distanceKm:  580
        9. Erstattung:  174,00 EUR (berechnet)

        Welches Feld aendern? (Nummer, oder "done")

User: 5
Claude: Aktueller Wert - purpose: Kundenpraesentation Q1
        Neuer Wert?

User: Projektvorstellung Q1
Claude: Geaendert: purpose "Kundenpraesentation Q1" -> "Projektvorstellung Q1"

        Weiteres Feld aendern? (Nummer, oder "done")

User: done
Claude: Gespeichert: ...\2026\02 - 2026\Max Mustermann\2026-02-03_berlin.md
        Geaendert: purpose
```

### With Search Argument

```
User: /trips:edit berlin
Claude: Gefunden: 2026-02-03_berlin.md

        [shows fields, same flow as above]
```

---

## What the Skill Does

1. Read `.trips/config.json`. If missing: error with hint to run `/trips:start`
2. Resolve dataDir from workspace root (cwd)
3. Identify trip to edit:
   - **No argument:** List trips of current month from `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`, user selects by number
   - **With argument:** Search trip files by filename or destination across all `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/` folders (case-insensitive)
   - If multiple matches: list them, user selects
4. Read the trip file, parse YAML frontmatter and body
5. Display all current field values (numbered)
6. User selects field(s) to change (loop until "done")
7. For each changed field:
   - Validate new value (same rules as `/trips:new`)
   - Update YAML frontmatter
   - If distanceKm changed: recalculate reimbursementEur
8. Regenerate body table from updated YAML (preserve Notizen section)
9. Handle file rename if needed:
   - If startDate or destination changed: rename file to match new `YYYY-MM-DD_<destination>.md`
   - If startDate month changed: move file to correct month folder
10. Write updated file

---

## Body Preservation

The Notizen section (everything below `## Notizen`) is **never modified** by this skill.

Regeneration scope:
- **Replaced:** Everything from start of body to (but not including) `## Notizen`
- **Preserved:** `## Notizen` heading and all content below it

If no `## Notizen` section exists, the skill appends one after regenerating the table.

---

## File Rename Logic

| Changed Field | Action |
|---------------|--------|
| startDate (same month) | Rename file: `YYYY-MM-DD_<dest>.md` |
| startDate (different month) | Move to new month's employee folder `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/` + rename |
| destination | Rename file: `YYYY-MM-DD_<dest>.md` |
| startDate + destination | Rename + possibly move |
| Other fields | No rename needed |

When moving files, create target employee folder (`<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/`) if it doesn't exist.

---

## Recalculation Rules

| Change | Effect |
|--------|--------|
| distanceKm updated | Recalculate reimbursementEur = distanceKm x 0,30 |
| distanceKm removed | Remove reimbursementEur from YAML |
| distanceKm added | Calculate and add reimbursementEur |
| Other fields | No recalculation |

---

## Notes

- Requires config (run `/trips:start` first)
- Multiple fields can be changed in one edit session (loop)
- Notizen section is always preserved
- File is renamed automatically when date or destination changes
- Erstattung is always recalculated from distanceKm, never edited directly
