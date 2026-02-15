---
name: "catalog"
description: "Move released song to 03_Catalog/ with catalog number"
---

# /music-production:catalog

## Purpose

Moves a release-ready song from `02_Vault/` to `03_Catalog/` with its assigned catalog number. This is the final step in the song lifecycle.

## When to Use

- Song has been released (uploaded to YouTube, Bandcamp, etc.)
- Ready to archive in the catalog with its permanent number

## Workflow

```
User: /music-production:catalog
Claude: Which song has been released?

        02_Vault/ (release-ready)
        1. 2026-02-Nightfall    [release] FW004

User: 1
Claude: Cataloging Nightfall as FW004...

        Moved: 02_Vault/2026-02-Nightfall/ -> 03_Catalog/FW004-Nightfall/
        Status: catalog
        Next catalog number: FW005

        Song is now permanently cataloged.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists
2. List songs in `02_Vault/` with `status: release`
3. User selects song
4. Read `catalog_nr` from `<Ordnername>.md` frontmatter
5. Determine target folder name: `[PREFIX][NR]-[Songtitel]`
   - Extract title from folder name (strip YYYY-MM- prefix)
   - Example: `2026-02-Nightfall` -> `FW004-Nightfall`
6. Move entire folder from `02_Vault/` to `03_Catalog/[PREFIX][NR]-[Songtitel]/`
7. Rename song file to match new folder name:
   - `2026-02-Nightfall.md` -> `FW004-Nightfall.md`
8. Update `<Ordnername>.md` frontmatter:
   - `status: release` -> `status: catalog`
   - Update `updated:` date
9. Increment `catalog.nextNumber` in `.music-production/config.json`

## Naming Convention

| Source | Target |
|--------|--------|
| `02_Vault/2026-02-Nightfall/` | `03_Catalog/FW004-Nightfall/` |
| `02_Vault/2025-11-Horizons/` | `03_Catalog/FW005-Horizons/` |
| `02_Vault/2026-01-Lost-In-Time/` | `03_Catalog/FW006-Lost-In-Time/` |

**Format:** `[PREFIX][NR (zero-padded to 3)]-[Title]`

---

## Transition Details

| Before | After |
|--------|-------|
| Location: `02_Vault/` | Location: `03_Catalog/` |
| Folder: `YYYY-MM-Title` | Folder: `PREFIX###-Title` |
| File: `YYYY-MM-Title.md` | File: `PREFIX###-Title.md` |
| `status: release` | `status: catalog` |
| Config `nextNumber: 4` | Config `nextNumber: 5` |

---

## Notes

- Only shows songs with `status: release` (must run `/music-production:release` first)
- Catalog number comes from song file frontmatter (assigned during release)
- `nextNumber` in config is incremented after successful move
- Folder is renamed from date-format to catalog-format
- All content (audio files, project files, song file) moves together
- Song file is renamed to match the new catalog folder name
