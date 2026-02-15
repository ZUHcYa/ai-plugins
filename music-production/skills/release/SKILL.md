---
name: "release"
description: "Prepare vault song for release - enrich song file with metadata and sections"
---

# /music-production:release

## Purpose

Picks a song from `02_Vault/` and enriches the `<Ordnername>.md` with release metadata (BPM, Key, Mood, description) and new sections for release texts and cover art.

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

User: Ein dunkler, atmosphaerischer Track mit schwebenden Synth-Pads
      und minimalistischen Beats. Inspiriert von der Stille kurz vor
      Sonnenuntergang. Melancholisch aber nicht traurig.

Claude: BPM?
User: 92

Claude: Key (Tonart)?
User: Dm

Claude: Mood (one word)?
User: melancholic

Claude: Release prepared!

        Updated: 02_Vault/2026-02-Nightfall/2026-02-Nightfall.md
        - Frontmatter: bpm: 92, key: Dm, mood: melancholic
        - Catalog-Nr: FW004
        - Added: Song-Beschreibung
        - Added: Release-Texte sections (empty - run /music-production:generate)
        - Added: Cover Art section (empty)

        Next: /music-production:generate to create release texts from your description.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists
2. List songs in `02_Vault/` with `status: vault`
3. User selects song
4. Interactively collect:
   - Song description (free text)
   - BPM (number)
   - Key/Tonart (e.g. Dm, C#m, G)
   - Mood (single word or short phrase)
5. Read `catalog.nextNumber` from config
6. Update `<Ordnername>.md` frontmatter:
   - `status: vault` -> `status: release`
   - Add `bpm`, `key`, `mood`, `catalog_nr`
   - Update `updated:` date
7. Add new sections to `<Ordnername>.md`:
   - `## Song-Beschreibung` (filled with user's description)
   - `## Release-Texte` (empty subsections for YouTube, Bandcamp)
   - `## Cover Art` (empty prompt and references)
8. Do NOT increment `nextNumber` yet (happens in `/music-production:catalog`)

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

**Single Source of Truth:** BPM, Key, Mood live exclusively in frontmatter. They are not repeated in the document body.

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

## Notes

- Song stays in `02_Vault/` during release prep (moves to catalog later)
- Catalog number is assigned but `nextNumber` is not incremented yet
- Description is free-form - the richer it is, the better `/music-production:generate` works
- Existing content (Referenzen, Notizen, TODOs) is preserved - new sections are appended
- Song file is named `<Ordnername>.md` (e.g. `2026-02-Nightfall.md`)
