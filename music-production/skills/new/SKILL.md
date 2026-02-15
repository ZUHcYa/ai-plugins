---
name: "new"
description: "Create a new song project in 01_Projects/"
---

# /music-production:new

## Purpose

Creates a new song project folder in `01_Projects/` with einer Song-Datei (`<Ordnername>.md`) from the canonical template.

## When to Use

- Starting work on a new song
- Capturing an initial idea with reference tracks

## Workflow

```
User: /music-production:new
Claude: What's the song title?

User: Nightfall
Claude: Any reference tracks? (optional, skip with Enter)

User: Radiohead - Everything In Its Right Place - die schwebende Atmosphaere
Claude: Created:
        01_Projects/2026-02-Nightfall/
        ├── 2026-02-Nightfall.md
        ├── 01_RAW/
        ├── 02_AUDIO/
        ├── 03_VIDEO/
        ├── 04_ARTWORK/
        └── 05_OUTPUT/

        Status: production
        Reference: 1 track added

        Open 2026-02-Nightfall.md to add more notes and TODOs as you produce.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists (if not: hint to run `/music-production:start`)
2. Ask for song title
3. Optionally ask for initial reference track(s) with notes
4. Create folder in `01_Projects/` with format `YYYY-MM-Songtitel`
   - YYYY-MM from current date
   - Songtitel in title case, spaces replaced with hyphens (no special chars)
5. Create Unterordner im Song-Folder:
   - `01_RAW/` - Rohmaterial (Aufnahmen, Samples, Field Recordings)
   - `02_AUDIO/` - Audio-Dateien (Bounces, Stems, Mixdowns)
   - `03_VIDEO/` - Video-Content
   - `04_ARTWORK/` - Cover Art, Visual Assets
   - `05_OUTPUT/` - Finale Dateien (Master, Release-Formate)
6. Create Song-Datei mit **exakt dem gleichen Namen wie der Ordner** + `.md`
   - Ordner heisst `2026-02-Nightfall` -> Datei heisst `2026-02-Nightfall.md`
   - Ordner heisst `2026-01-Lost-In-Time` -> Datei heisst `2026-01-Lost-In-Time.md`
7. Fill in title, date, and optional references

> **CRITICAL - Dateibenennung:** Die Song-Datei traegt IMMER exakt den Ordnernamen + `.md`. Beispiel: Ordner `2026-02-Nightfall/` -> Datei `2026-02-Nightfall.md`. NIEMALS `_song.md`, `song.md` oder andere generische Namen verwenden.

## Naming Convention

| Part | Format | Example |
|------|--------|---------|
| Date prefix | `YYYY-MM` | `2026-02` |
| Separator | `-` | `-` |
| Title | Title-Case, hyphens | `Nightfall`, `Lost-In-Time` |
| Full | Combined | `2026-02-Nightfall` |

**Rules:**
- No special characters (umlauts, accents) - use ASCII equivalents
- No spaces - use hyphens
- Title case for readability

---

## Song Template

Canonical template for new songs in Production phase:

```markdown
---
tags:
  - song
type: song
genre:
status: production
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# [Songtitel]

## Referenzen
*Verlinke Tracks mit kurzer Notiz, was du davon willst:*

---

## Notizen
*Ideen, Sounds, Konzept - was auch immer*

## TODOs
- [ ]
```

### Template Field Reference

| Field | Set by Skill | User-editable |
|-------|-------------|---------------|
| `tags` | `song` (default) | Yes - add more tags |
| `type` | `song` (fixed) | No |
| `genre` | Empty | Yes - fill during production |
| `status` | `production` | No - managed by skills |
| `created` | Current date | No |
| `updated` | Current date | Auto-updated by skills |

---

## Notes

- Requires config (run `/music-production:start` first)
- Creates folder, 5 Unterordner AND Song-Datei in one step
- Song-Datei heisst IMMER wie der Ordner (z.B. Ordner `2026-02-Nightfall/` -> Datei `2026-02-Nightfall.md`)
- Template is intentionally minimal - grows during production
- References use `[[wiki-link]]` format for Obsidian compatibility (works as plain text in other editors)
