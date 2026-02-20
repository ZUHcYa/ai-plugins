---
name: "list"
description: "List and filter recorded trips"
---

# /trips:list

## Purpose

Displays recorded trips with optional filtering by year, month, or destination.
Reads YAML frontmatter from trip files as the source of truth.

## When to Use

- Quick overview of trips in a specific period
- Finding trips to a specific destination
- Checking what has been recorded before creating a report

## Workflow

### No Filter (Current Year)

```
User: /trips:list
Claude: Dienstreisen 2026
        ======================================

        Januar (3 Reisen, 8 Tage, 1.200 km, 360,00 EUR)
        | Datum | Reise | Ziel | Zweck | km | EUR |
        |-------|-------|------|-------|----|-----|
        | 12.01. | Kickoff Frankfurt | Frankfurt | Projekt-Kickoff | 420 | 126,00 |
        | 15.-16.01. | Messe Stuttgart | Stuttgart | CES Besuch | 380 | 114,00 |
        | 28.01. | Kundenbesuch Hamburg | Hamburg | Quarterly Review | 400 | 120,00 |

        Februar (2 Reisen, 3 Tage, 580 km, 174,00 EUR)
        | Datum | Reise | Ziel | Zweck | km | EUR |
        |-------|-------|------|-------|----|-----|
        | 03.02. | Kundentermin Berlin | Berlin | Praesentation Q1 | 580 | 174,00 |
        | 10.-12.02. | Workshop Muenchen | Muenchen | Team Workshop | - | - |

        Gesamt 2026: 5 Reisen, 11 Reisetage, 1.780 km, 534,00 EUR
```

### Filter by Year

```
User: /trips:list 2025
Claude: Dienstreisen 2025
        ======================================
        [Shows all 2025 trips grouped by month]
```

### Filter by Month

```
User: /trips:list 2026-02
Claude: Dienstreisen Februar 2026
        ======================================

        | Datum | Reise | Ziel | Zweck | km | EUR |
        |-------|-------|------|-------|----|-----|
        | 03.02. | Kundentermin Berlin | Berlin | Praesentation Q1 | 580 | 174,00 |
        | 10.-12.02. | Workshop Muenchen | Muenchen | Team Workshop | - | - |

        Gesamt: 2 Reisen, 3 Reisetage, 580 km, 174,00 EUR
```

### Filter by Destination

```
User: /trips:list Berlin
Claude: Dienstreisen nach "Berlin"
        ======================================

        | Datum | Reise | Zweck | km | EUR |
        |-------|-------|-------|----|-----|
        | 03.02.2026 | Kundentermin Berlin | Praesentation Q1 | 580 | 174,00 |
        | 15.06.2025 | Workshop Berlin | Strategie Meeting | 580 | 174,00 |

        Gesamt: 2 Reisen nach Berlin, 1.160 km, 348,00 EUR
```

---

## What the Skill Does

1. Read `.trips/config.json`. If missing: error with hint to run `/trips:start`
2. Resolve dataDir from workspace root (cwd)
3. Parse optional argument:
   - No argument: current year
   - `YYYY`: specific year
   - `YYYY-MM`: specific month
   - Text string: destination filter (search across all years)
4. Read relevant trip files from `<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/` (skip `_report-*.md` files), parse YAML frontmatter
5. Sort trips chronologically (oldest first)
6. Display trips in table format grouped by month
7. Show summary totals

---

## Argument Parsing

| Input | Interpretation |
|-------|---------------|
| (empty) | Current year |
| `2025` | Year 2025 |
| `2026-02` | February 2026 |
| `Berlin` | All trips with destination containing "Berlin" |

**Heuristic:** If input matches `YYYY` or `YYYY-MM` pattern, treat as date filter.
Otherwise, treat as destination filter (case-insensitive substring match).

---

## Display Rules

- Group by month when showing a full year
- Sort chronologically (oldest first within display)
- Datum column uses short format within year view (`DD.MM.` or `DD.-DD.MM.`)
- Datum column uses full date for cross-year destination results (`DD.MM.YYYY`)
- Omit km and EUR columns entirely if no trips in the result set have distanceKm
- Show totals for the filtered result set

---

## Notes

- Read-only skill - never modifies files
- Always reads YAML frontmatter as source of truth
- Destination filter is case-insensitive substring match
- Shows totals for the filtered result set
- km and EUR columns adapt based on available data
