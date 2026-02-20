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

        02_Vault/ (status: release)
        1. 2026-02-Nightfall    [release] FW004

User: 1
Claude: Cataloging Nightfall as FW004...

        Moved:    02_Vault/2026-02-Nightfall/ -> 03_Catalog/FW004-Nightfall/
        Renamed:  2026-02-Nightfall.md -> FW004-Nightfall.md
        Status:   catalog
        Config:   nextNumber updated to FW005

        Song is now permanently cataloged.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists. If not: prompt user to run `/music-production:start` first and stop.
2. List songs in `02_Vault/` with `status: release`.
3. User selects song.
4. Read `catalog_nr` from `<FolderName>.md` frontmatter (e.g. `FW004`).
5. Check for catalog number sync issues (see edge cases below).
6. Determine target folder name: `[PREFIX][NR]-[Title]`
   - Extract title from source folder name by stripping the `YYYY-MM-` prefix
   - Example: `2026-02-Nightfall` -> `FW004-Nightfall`
7. Move entire folder from `02_Vault/` to `03_Catalog/[PREFIX][NR]-[Title]/`.
8. Rename the song file inside the moved folder to match the new folder name:
   - `2026-02-Nightfall.md` -> `FW004-Nightfall.md`
9. Update the song file frontmatter:
   - `status: release` -> `status: catalog`
   - Update `updated:` date
10. Increment `catalog.nextNumber` in `.music-production/config.json`.

## Naming Convention

| Source | Target |
|--------|--------|
| `02_Vault/2026-02-Nightfall/` | `03_Catalog/FW004-Nightfall/` |
| `02_Vault/2025-11-Horizons/` | `03_Catalog/FW005-Horizons/` |
| `02_Vault/2026-01-Lost-In-Time/` | `03_Catalog/FW006-Lost-In-Time/` |

**Format:** `[PREFIX][NR (zero-padded to 3)]-[Title]`

The song file inside the folder is renamed to match the new folder name. All other files (audio, project files, artwork) remain unchanged.

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

## Edge Cases

### Catalog Number Gaps

If the scan of `03_Catalog/` reveals gaps in the numbering (e.g. FW001, FW003 exist but FW002 is missing):

```
Claude: Warning: Gap detected in catalog numbers.
        Found in 03_Catalog/: FW001, FW003
        Missing: FW002

        This may mean FW002 was deleted, renamed manually, or never
        assigned. The next number from config is: FW004.

        Proceeding with FW004 as configured. No attempt to fill the gap.
        If FW002 should be recovered, handle it manually.
```

The skill always uses `catalog.nextNumber` from config as the authoritative source — never infers the next number by scanning existing files. Gaps are warnings only, not blockers.

### Config nextNumber Out of Sync

If the song's assigned `catalog_nr` (set during `/music-production:release`) does not match the current `catalog.nextNumber` in config:

```
Claude: Sync warning: "Nightfall" was assigned FW004 during release prep,
        but config.nextNumber is currently FW006.

        This can happen if catalog numbers were assigned but songs were
        not yet cataloged, or if config was edited manually.

        Options:
        [1] Use FW004 (from song frontmatter — keeps the original assignment)
        [2] Use FW006 (from config — reassigns the catalog number)
        [3] Cancel

User: 1
Claude: Using FW004 as assigned. Config nextNumber will be left at FW006.
        Note: FW005 is now a gap in the catalog.
```

The user must explicitly choose. Never silently reassign a catalog number.

### Catalog Number Already Taken

If `03_Catalog/FW004-*` already exists (another song was already cataloged with FW004):

```
Claude: Error: FW004 is already used in 03_Catalog/ (FW004-Echoes/).

        The catalog number FW004 in "Nightfall" conflicts with an
        existing entry. This should not happen under normal use.

        Options:
        [1] Assign next available number (FW007)
        [2] Cancel and resolve manually

User: 2
```

Never overwrite an existing catalog entry.

### Song File Rename Conflict

If a file named `FW004-Nightfall.md` already exists inside the folder after the move:

```
Claude: Warning: A file named "FW004-Nightfall.md" already exists in
        the moved folder. Skipping rename to avoid data loss.
        Rename manually: 2026-02-Nightfall.md -> FW004-Nightfall.md
```

### No Songs with `status: release`

```
Claude: No songs found in 02_Vault/ with status: release.

        Run /music-production:release to prepare a song for release first.
```

---

## Config nextNumber Integrity

The `catalog.nextNumber` field in `.music-production/config.json` is incremented only after a successful catalog operation. If the move or rename fails partway through, the number is NOT incremented — the user must retry or resolve manually.

After a successful catalog operation:

```json
{
  "catalog": {
    "prefix": "FW",
    "nextNumber": 5
  }
}
```

---

## Notes

- Only shows songs with `status: release` (must run `/music-production:release` first)
- Catalog number comes from song file frontmatter (assigned during `/music-production:release`)
- `nextNumber` in config is incremented after successful move — never before
- Folder is renamed from date-format to catalog-format; all content moves together
- Song file is renamed to match the new catalog folder name
- All content (audio files, project files, artwork) moves without modification
- Gaps in catalog numbers are not automatically filled — use `nextNumber` from config as the source of truth
