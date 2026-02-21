# music-production Quick-Start Guide

Get started with music-production in 5 minutes.

---

## Installation

### Option A: Claude Code Plugin (Recommended)

**Step 1:** Add the marketplace
```
/plugin marketplace add ZUHcYa/ai-plugins
```

**Step 2:** Install the plugin
```
/plugin install music-production@ai-plugins
```

**Step 3:** Run setup in your Music-Production root folder
```
/music-production:start
```

### Option B: Manual Setup

1. Create the folder structure: `01_Projects/`, `02_Vault/`, `03_Catalog/`
2. Create `.music-production/config.json` with your catalog prefix and next number
3. Create song folders manually using the naming convention: `YYYY-MM-Songtitel`

### Updates

To get the latest version, pull the latest changes from the repository or re-install the plugin.

---

## 1. Setup

```
/music-production:start
```

On first run, music-production detects your folder structure and asks for a catalog prefix:

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

**Note:** The plugin does NOT create the folder structure — it must already exist.

See [STRUCTURE.md](STRUCTURE.md) for complete folder documentation.

---

## 2. Create a New Song

```
/music-production:new
```

Creates a new song folder in `01_Projects/` with naming convention `YYYY-MM-Songtitel`:

```
User: /music-production:new
Claude: What's the song title?

User: Nightfall

Claude: Created: 01_Projects/2026-02-Nightfall/
                          2026-02-Nightfall.md  <- main song file

        Open 2026-02-Nightfall.md and fill in your notes.
        Run /music-production:vault when the song is finished.
```

---

## 3. Get Production Help (Optional)

```
/music-production:produce
```

Need help finishing a song? Get advisory consultation on arrangement, harmony, sound design, mixing, or mastering:

```
User: /music-production:produce
Claude: An welchem Song arbeitest du?

        01_Projects/
        1. 2026-02-Nightfall    [production] - 3 Tage

        [S] Skip - kein bestimmter Song / allgemeine Frage

User: 1
Claude: Woran moechtest du arbeiten?

        [A] Arrangement  [H] Harmony & Theory  [S] Sound Design
        [R] Rhythm & Groove  [M] Mixing  [X] Mastering

        Oder beschreib dein Problem in eigenen Worten.

User: Der Chorus klingt duenn
Claude: [Konkrete Vorschlaege basierend auf Song-Kontext und Referenzen]

        Action Items in Song-TODOs uebernehmen? [J/n]
```

---

## 4. Move to Vault When Finished

```
/music-production:vault
```

When production is complete, move the song to `02_Vault/` and mark it as waiting for release.

---

## 5. Prepare for Release

```
/music-production:release
```

Enriches the song file with release metadata (BPM, Key, Mood, genre tags). Run this when you're ready to publish.

```
/music-production:generate
```

Generates release texts for YouTube, Bandcamp descriptions, and a cover art prompt. Uses the enriched metadata from the song file.

---

## 6. Catalog the Song

```
/music-production:catalog
```

Moves the song to `03_Catalog/` and assigns the next catalog number:

```
User: /music-production:catalog
Claude: Assigned: FW004

        Moved to: 03_Catalog/FW004-Nightfall/
                              FW004-Nightfall.md

        Next catalog number: FW005 (saved to config)
```

---

## The Complete Lifecycle

```
01_Projects/  ->  02_Vault/  ->  Release-Prep  ->  03_Catalog/
     |               |              |               |
  /mp:new          /mp:vault     /mp:release      /mp:catalog
  /mp:produce                    /mp:generate
  (advisory)
```

---

## Check Your Status

Run `/music-production:start` anytime to see all songs across all phases:

```
User: /music-production:start
Claude: Music-Production Status (v0.3.0)
        ======================================

        01_Projects (2 active)
        -----------------------
        2026-02-Nightfall     [production] - 3 days
        2026-01-Echoes        [production] - 35 days stale

        02_Vault (1 waiting)
        ---------------------
        2025-12-Horizons      [vault] - ready for release

        03_Catalog (3 released)
        ------------------------
        FW001-Funereal
        FW002-Patchworz
        FW003-Lionheart

        Suggested Actions
        -----------------
        1. Echoes is 35 days old -> finish or archive
        2. Horizons in vault -> /music-production:release
        3. Next catalog number: FW004
```

---

## Helpful Skills

| Skill | When to use |
|-------|-------------|
| `/music-production:start` | Setup (first run) or status overview |
| `/music-production:new` | Start a new song project |
| `/music-production:produce` | Production consultation (arrangement, harmony, mixing, mastering) |
| `/music-production:vault` | Song is finished, move to waiting |
| `/music-production:release` | Add release metadata |
| `/music-production:generate` | Generate release texts |
| `/music-production:catalog` | Assign catalog number, archive |

---

## Manual Workflow (No Skills)

music-production works without Claude Code skills too:

1. Create folder: `01_Projects/2026-02-Mysong/`
2. Create file: `01_Projects/2026-02-Mysong/2026-02-Mysong.md` (file must be named exactly like its folder)
3. Add frontmatter: `status: production`, `created: 2026-02-20`
4. When done: move folder to `02_Vault/`, update `status: vault`
5. After release prep: move folder to `03_Catalog/`, rename to `FW004-Mysong/`

All files are standard Markdown — use any editor (Obsidian, VS Code, Finder, etc.).
