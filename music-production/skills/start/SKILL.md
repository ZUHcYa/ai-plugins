---
name: "start"
description: "Smart entry point - setup on first run, overview afterwards"
---

# /music-production:start

## Purpose

Single entry point for music-production. Automatically detects context:
- **First run:** Sets up config for the current Music-Production folder
- **After setup:** Shows combined overview across all phases

## Phase Lifecycle

```
01_Projects/     ->  02_Vault/      ->  Release-Prep     ->  03_Catalog/
Produktion          Fertig, wartet      Texte & Meta          Veroeffentlicht

/mp:new             /mp:vault           /mp:release           /mp:catalog
                                        /mp:generate
```

## When to Use

- Starting with music-production for the first time
- Beginning a session - get oriented quickly
- After a break - see where you left off
- Quick status check across all phases

---

## First Run (No Config)

When no `.music-production/config.json` exists, setup runs automatically:

```
User: /music-production:start
Claude: Welcome to music-production!

        Detected folder structure:
        - 01_Projects/
        - 02_Vault/
        - 03_Catalog/

        What's your catalog prefix? (e.g. "FW" for FW001, FW002...)

User: FW
Claude: Setting up...

        Scanned 03_Catalog/ - found FW003 as highest entry.
        Next catalog number: FW004

        Created: .music-production/config.json

        Ready! Run /music-production:new to start a new song.
```

### Missing Folder Structure

If the expected folders are not found:

```
User: /music-production:start
Claude: I can't find the expected folder structure.

        Expected:
        - 01_Projects/
        - 02_Vault/
        - 03_Catalog/

        Are you in your Music-Production root folder?
```

The plugin does NOT create the folder structure - it must already exist.

---

## After Setup (Config Exists)

When config exists, shows combined overview:

```
User: /music-production:start
Claude: Music-Production Status (v0.3.0)
        ======================================

        01_Projects (3 active)
        -----------------------
        2026-01-Nightfall     [production] - 12 days
        2026-02-Echoes        [production] - 3 days
        2025-12-Drift         [production] - 48 days stale

        02_Vault (2 waiting)
        ---------------------
        Horizons              [vault] - ready for release
        Embers                [vault] - ready for release

        03_Catalog (3 released)
        ------------------------
        FW001-Funereal
        FW002-Patchworz
        FW003-Lionheart

        Suggested Actions
        -----------------
        1. Drift is 48 days old -> finish or archive
        2. 2 songs in vault -> /music-production:release
        3. Next catalog number: FW004
```

### Empty Project

```
User: /music-production:start
Claude: Music-Production Status (v0.3.0)
        ======================================

        01_Projects (0)
        02_Vault (0)
        03_Catalog (0)

        Get Started
        -----------
        Run /music-production:new to start your first song!
```

---

## What the Skill Does

### First Run (Setup)

1. Check if `.music-production/config.json` exists
2. If not: verify folder structure (`01_Projects/`, `02_Vault/`, `03_Catalog/`)
3. If structure missing: warn user and stop
4. Ask for catalog prefix
5. Scan `03_Catalog/` for existing entries, determine highest number
6. Create `.music-production/config.json`

### After Setup (Overview)

1. Read version from `music-production/.claude-plugin/plugin.json` (= installed version)
2. Version-Check: WebFetch `https://raw.githubusercontent.com/ZUHcYa/ai-hub/main/music-production/.claude-plugin/plugin.json`, JSON parsen, `version` Feld extrahieren (= latest version).
   - Wenn latest > installed: im Header anzeigen: `Music-Production Status (v0.x.0 - Update verfuegbar: v0.y.0)` und am Ende "Update: Plugin neu installieren fuer vX.Y.Z"
   - Wenn gleich oder Fetch fehlschlaegt: nur `Music-Production Status (v0.x.0)` zeigen
3. Read config from `.music-production/config.json`
4. Scan phase folders:
   - `01_Projects/` - list all song folders, read `<Ordnername>.md` frontmatter
   - `02_Vault/` - list all song folders, read `<Ordnername>.md` frontmatter
   - `03_Catalog/` - list all catalog entries
5. Calculate staleness (days since folder creation/modification)
6. Display status grouped by phase
7. Generate actionable suggestions

---

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| Song in 01_Projects/ > 30 days | "X is N days old -> finish or archive" |
| Songs exist in 02_Vault/ | "N songs in vault -> /music-production:release" |
| Song in Vault with `status: release` | "X ready -> /music-production:catalog" |
| No songs exist | "Run /music-production:new to start your first song" |

---

## Configuration File

**Location:** `.music-production/config.json`

```json
{
  "version": "1.0",
  "created": "YYYY-MM-DD",
  "targetPath": "./",
  "catalog": {
    "prefix": "FW",
    "nextNumber": 4
  }
}
```

| Field | Purpose |
|-------|---------|
| `version` | Config schema version |
| `created` | Setup date |
| `targetPath` | Root of Music-Production folder (default `"./"`) |
| `catalog.prefix` | User-defined prefix for catalog numbers |
| `catalog.nextNumber` | Next available catalog number (auto-detected on setup) |

---

## Notes

- Single command to remember: just `/music-production:start`
- Context-aware: detects first run vs. returning user
- Plugin does NOT create the Music-Production folder structure
- Actionable suggestions help users know what to do next
