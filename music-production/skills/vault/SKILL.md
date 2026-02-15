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

1. Check that `.music-production/config.json` exists
2. List all song folders in `01_Projects/`
3. Read `<Ordnername>.md` frontmatter from each
4. User selects which song to move
5. Move entire folder from `01_Projects/` to `02_Vault/`
6. Update `<Ordnername>.md` frontmatter:
   - `status: production` -> `status: vault`
   - `updated:` -> current date

## Transition Details

| Before | After |
|--------|-------|
| Location: `01_Projects/` | Location: `02_Vault/` |
| `status: production` | `status: vault` |
| Content unchanged | Content unchanged |

The vault is a holding area. No content changes to the song file - it simply waits to be picked for release.

---

## Notes

- Moves the entire folder (not just the song file)
- Only updates status and date in frontmatter
- No content modifications
- User can always move folders manually - this skill just automates it
