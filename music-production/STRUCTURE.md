# music-production Folder Structure

Overview of the music-production folder organization.

---

## The Three-Phase Model

Songs move through three folder stages as they progress from idea to catalog.

| Folder | Phase | Purpose |
|--------|-------|---------|
| `01_Projects/` | Production | Active songs being produced |
| `02_Vault/` | Waiting | Finished songs waiting for release prep |
| `03_Catalog/` | Published | Released songs with catalog numbers |

**Important:** The plugin does NOT create this structure. You must create these folders in your Music-Production root before running `/music-production:start`.

---

## Song Folder Convention

Each song lives in its own folder. The folder name is also the song file name.

**Naming pattern:**
- In `01_Projects/` and `02_Vault/`: `YYYY-MM-Songtitel/`
- In `03_Catalog/`: `PREFIX###-Songtitel/` (e.g. `FW004-Nightfall/`)

**Critical rule:** The song file inside the folder MUST be named exactly like its folder:
```
01_Projects/
└── 2026-02-Nightfall/
    └── 2026-02-Nightfall.md    <- must match folder name exactly
```

After cataloging:
```
03_Catalog/
└── FW004-Nightfall/
    └── FW004-Nightfall.md      <- renamed to match new folder name
```

---

## Song File Structure

Each song file is a Markdown file with YAML frontmatter as the single source of truth:

```markdown
---
title: Nightfall
created: 2026-02-20
status: production
bpm: 128
key: Am
mood: melancholic
catalog_nr:
---

# Nightfall

## Notes

...
```

**Frontmatter is the single source of truth.** The Markdown body is for human-readable notes and generated release texts.

---

## Complete Structure

```
<Music-Production Root>/
├── .music-production/
│   └── config.json          # Catalog prefix and next number
├── 01_Projects/             # Active songs in production
│   └── 2026-02-Nightfall/
│       └── 2026-02-Nightfall.md
├── 02_Vault/                # Finished, waiting for release
│   └── 2025-12-Horizons/
│       └── 2025-12-Horizons.md
├── 03_Catalog/              # Released songs
│   └── FW003-Lionheart/
│       └── FW003-Lionheart.md
├── skills/                  # Plugin skill documentation
│   ├── start/SKILL.md
│   ├── new/SKILL.md
│   ├── vault/SKILL.md
│   ├── release/SKILL.md
│   ├── generate/SKILL.md
│   └── catalog/SKILL.md
├── CLAUDE.md
├── README.md
├── QUICKSTART.md
├── STRUCTURE.md             # This file
└── CHANGELOG.md
```

---

## Configuration

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

The `catalog.nextNumber` is automatically incremented after each `/music-production:catalog` run.

---

## Internal Song Folder Structure (Optional)

Some songs may have sub-folders for production assets. These are not managed by the plugin but follow a common convention:

```
2026-02-Nightfall/
├── 2026-02-Nightfall.md     # Main song file (required)
├── 01_RAW/                  # DAW projects, raw recordings
├── 02_AUDIO/                # Mixed/mastered audio files
├── 03_VIDEO/                # Video exports
├── 04_ARTWORK/              # Cover art files
└── 05_OUTPUT/               # Final output files
```

The plugin only reads the main `.md` file — the sub-folders are for your own organization.

---

**Version:** 0.3.0
