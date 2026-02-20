---
name: "vault"
description: "Move finished song from 01_Projects/ to 02_Vault/"
---

# /music-production:vault

## Purpose

Moves a finished production from `01_Projects/` to `02_Vault/`. The song is done but not yet picked for release.

## When to Use

- Song production is finished (mix/master done)
- Ready to park the song until it gets picked for a release

## Workflow

```
User: /music-production:vault
Claude: Which song is finished?

        01_Projects/
        1. 2026-02-Nightfall    [production] - 3 days
        2. 2026-01-Echoes       [production] - 28 days
        3. 2025-12-Drift        [production] - 48 days

User: 1
Claude: Moving 2026-02-Nightfall to vault...

        Moved: 01_Projects/2026-02-Nightfall/ -> 02_Vault/2026-02-Nightfall/
        Status: vault

        Song is now in the vault, waiting to be picked for release.
        Run /music-production:release when ready.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists. If not: prompt user to run `/music-production:start` first and stop.
2. Check that `01_Projects/` and `02_Vault/` exist. If either is missing, warn the user and stop.
3. Scan `01_Projects/` for song folders.
4. For each folder, look for `<FolderName>.md`. If the song file is missing, show the folder with a warning indicator (see edge cases below).
5. Display the list with status and age.
6. User selects which song to move.
7. Confirm the move with the user if the song is very recent (less than 1 day old) — this may be accidental.
8. Move the entire folder from `01_Projects/` to `02_Vault/`.
9. Update the `<FolderName>.md` frontmatter:
   - `status: production` -> `status: vault`
   - `updated:` -> current date

## Transition Details

| Before | After |
|--------|-------|
| Location: `01_Projects/` | Location: `02_Vault/` |
| `status: production` | `status: vault` |
| Content unchanged | Content unchanged |

The vault is a holding area. No content changes to the song file — it simply waits to be picked for release.

---

## Edge Cases

### Song Already in Vault

If a folder with the same name already exists in `02_Vault/`:

```
Claude: "2026-02-Nightfall" already exists in 02_Vault/.

        This song appears to have been moved before.
        No action taken. Check 02_Vault/2026-02-Nightfall/ manually.
```

The skill never overwrites an existing vault entry silently. The user must resolve the conflict manually.

### Song File Missing

If a folder exists in `01_Projects/` but the expected `<FolderName>.md` file is not found:

```
Claude: Which song is finished?

        01_Projects/
        1. 2026-02-Nightfall    [production] - 3 days
        2. 2026-01-Echoes       [NO SONG FILE] - 28 days   <- warning
        3. 2025-12-Drift        [production] - 48 days

User: 2
Claude: Warning: No song file found in 01_Projects/2026-01-Echoes/.
        Expected: 2026-01-Echoes.md

        The folder can still be moved, but the status field in frontmatter
        cannot be updated automatically.

        Proceed anyway? [y/N]

User: y
Claude: Moved: 01_Projects/2026-01-Echoes/ -> 02_Vault/2026-01-Echoes/
        Note: Create 2026-01-Echoes.md manually and set status: vault
```

This allows the move to proceed even in an inconsistent state, while making the user aware of the issue.

### Empty `01_Projects/` Folder

If no song folders exist:

```
Claude: No songs found in 01_Projects/.

        Create a new song with /music-production:new.
```

### Staleness Warning

Songs older than 30 days in `01_Projects/` are flagged as stale in the listing:

```
        3. 2025-12-Drift        [production] - 48 days  (stale)
```

This is informational only — the user can vault any song regardless of age.

---

## Notes

- Moves the entire folder (not just the song file)
- Only updates status and date in frontmatter — no content modifications
- User can always move folders manually; this skill just automates the status update
- After vaulting, run `/music-production:release` to prepare the song for distribution
