---
name: "release"
description: "Prepare vault song for release - enrich song file with metadata and sections"
---

# /music-production:release

## Purpose

Picks a song from `02_Vault/` and enriches the `<FolderName>.md` with release metadata (BPM, Key, Mood, description) and new sections for release texts and cover art.

## When to Use

- A song has been picked for release (single, album, etc.)
- Ready to prepare all metadata needed for distribution

## Workflow

```
User: /music-production:release
Claude: Which song do you want to release?

        02_Vault/
        1. 2026-02-Nightfall    [vault]
        2. 2025-11-Horizons     [vault]

User: 1
Claude: Tell me about "Nightfall" - describe the song's mood, story,
        and influences in a few sentences:

User: A dark, atmospheric track with floating synth pads and minimalist
      beats. Inspired by the silence just before sunset. Melancholic
      but not sad.

Claude: BPM?
User: 92

Claude: Key?
User: Dm

Claude: Genre?
User: electronic

Claude: Mood (one word or short phrase)?
User: melancholic

Claude: Release prepared!

        Updated: 02_Vault/2026-02-Nightfall/2026-02-Nightfall.md
        - Frontmatter: bpm: 92, key: Dm, genre: electronic, mood: melancholic
        - Catalog number: FW004
        - Added: Song description
        - Added: Release texts sections (empty - run /music-production:generate)
        - Added: Cover Art section (empty)

        Next: /music-production:generate to create release texts from your description.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists. If not: prompt user to run `/music-production:start` first and stop.
2. List songs in `02_Vault/` with `status: vault`.
3. User selects song.
4. Check if release metadata already exists (see edge case below).
5. Interactively collect:
   - Song description (free text — the richer this is, the better `/music-production:generate` works)
   - BPM (number)
   - Key (e.g. Dm, C#m, G)
   - Genre (e.g. electronic, ambient, techno)
   - Mood (single word or short phrase, e.g. "melancholic", "euphoric", "tense")
6. Read `catalog.nextNumber` from config to assign the catalog number.
7. Update `<FolderName>.md` frontmatter:
   - `status: vault` -> `status: release`
   - Add `bpm`, `key`, `genre`, `mood`, `catalog_nr`
   - Update `updated:` date
8. Append new sections to `<FolderName>.md` (preserve existing content):
   - `## Song-Beschreibung` — filled with the user's description
   - `## Release-Texte` — empty subsections for YouTube and Bandcamp
   - `## Cover Art` — empty prompt and style references
9. Do NOT increment `nextNumber` in config (that happens in `/music-production:catalog`).

## Metadata Fields

| Field | Type | Example | Purpose |
|-------|------|---------|---------|
| `bpm` | Number | `92` | Tempo — used in YouTube description and Bandcamp metadata |
| `key` | String | `Dm` | Musical key — used in YouTube description |
| `genre` | String | `electronic` | Primary genre — used for Bandcamp tags and distribution |
| `mood` | String | `melancholic` | Emotional quality — used in cover art prompt and release texts |
| `catalog_nr` | String | `FW004` | Permanent catalog number assigned at this stage |

**Single Source of Truth:** BPM, Key, Genre, and Mood live exclusively in frontmatter. They are not repeated in the document body.

## Frontmatter After Release

```yaml
---
tags:
  - song
type: song
genre: electronic
status: release
created: 2026-02-01
updated: 2026-02-08
bpm: 92
key: Dm
mood: melancholic
catalog_nr: FW004
---
```

---

## Release Sections Template

These sections are appended to the existing song file content:

```markdown

---

## Song-Beschreibung

[User's free-text description goes here]

## Release-Texte

### YouTube
**Title:**
**Description:**

### Bandcamp
**Title:**
**Description:**
**Tags:**

## Cover Art
**Prompt:**
**Stil-Referenzen:**
```

---

## Edge Cases

### Metadata Already Present

If `bpm`, `key`, `mood`, or `catalog_nr` already exist in the frontmatter (e.g. release was started before but not completed):

```
Claude: "2026-02-Nightfall" already has partial release metadata:
        bpm: 92 | key: Dm | mood: (missing) | catalog_nr: FW004

        Options:
        [1] Fill in missing fields only (keep existing values)
        [2] Re-enter all metadata (overwrite)
        [3] Cancel

User: 1
Claude: mood (missing)?
User: melancholic
Claude: Release metadata complete.
```

Never silently overwrite existing metadata. The user must explicitly choose to overwrite.

### Release Sections Already Present

If `## Release-Texte` or `## Cover Art` sections already exist in the song file (e.g. added manually or by a previous partial run):

```
Claude: Release sections already exist in 2026-02-Nightfall.md.
        Skipping section append to avoid duplicates.

        Run /music-production:generate to (re)generate the release texts.
```

### Song File Missing

If the expected `<FolderName>.md` does not exist in the vault folder:

```
Claude: Cannot find the song file: 02_Vault/2026-02-Nightfall/2026-02-Nightfall.md

        Create the file manually and ensure it has valid YAML frontmatter,
        then re-run /music-production:release.
```

### No Songs in Vault

If `02_Vault/` is empty or contains no songs with `status: vault`:

```
Claude: No songs found in 02_Vault/ with status: vault.

        To move a finished song to the vault, run /music-production:vault.
```

---

## Notes

- Song stays in `02_Vault/` during release preparation (moves to `03_Catalog/` later via `/music-production:catalog`)
- Catalog number is assigned here but `nextNumber` is not incremented until catalog
- Description is free-form — the richer it is, the better `/music-production:generate` works
- Existing content (Referenzen, Notizen, TODOs) is preserved — new sections are appended
- Song file is named `<FolderName>.md` (e.g. `2026-02-Nightfall.md`)
