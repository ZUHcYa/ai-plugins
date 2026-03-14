# music-production - Music Production Workflow Companion

A lifecycle companion for music production that guides songs from idea to catalog.

## What It Does

**music-production** manages your song lifecycle through structured phases:

- **Defining** your artist identity, distribution strategy, and production philosophy
- **Creating** new song projects with consistent templates
- **Advising** on production decisions with identity-aware consultation
- **Moving** finished productions to vault for release preparation
- **Enriching** songs with metadata, release texts, and cover art prompts
- **Archiving** released songs to your catalog with sequential numbering

## Quick Start

```
/music-production:start       Setup on first run, overview afterwards
/music-production:strategy    Define artist identity, distribution & production philosophy
/music-production:new         Create a new song project
/music-production:produce     Production consultation (identity-aware)
/music-production:vault       Move finished song to vault
/music-production:release     Prepare song for release (metadata + optional video/artwork)
/music-production:generate    Generate release texts and cover art prompt (strategy-aware)
/music-production:catalog     Archive to catalog with catalog number
```

## Phase Lifecycle

```
01_Projects/     ->  02_Vault/      ->  Release-Prep     ->  03_Catalog/
Production           Finished           Texts & Meta          Published

/music-production:new      /music-production:vault   /music-production:release   /music-production:catalog
/music-production:produce                            /music-production:generate
(advisory)
```

## Skills

| Skill | Purpose |
|-------|---------|
| `/music-production:start` | Smart entry point - setup on first run, overview afterwards |
| `/music-production:strategy` | Define artist identity, distribution strategy, production philosophy |
| `/music-production:new` | Create a new song project in `01_Projects/` |
| `/music-production:produce` | Production consultation - identity-aware advice on arrangement, harmony, mixing, mastering |
| `/music-production:vault` | Move finished song from `01_Projects/` to `02_Vault/` |
| `/music-production:release` | Enrich song file with metadata (BPM, Key, Mood) and optional video/artwork status |
| `/music-production:generate` | Generate YouTube/Bandcamp descriptions and cover art prompt (strategy-aware) |
| `/music-production:catalog` | Move released song to `03_Catalog/` with catalog number (e.g. FW004) |

## Folder Structure

The plugin expects this structure to already exist in your music production root:

```
Music-Root/
├── .music-production/           Plugin config (created by /music-production:start)
│   ├── config.json              Catalog prefix and next number
│   └── strategy.md              Artist identity (created by /music-production:strategy)
├── 01_Projects/                 Active songs in production
│   └── 2026-02-Nightfall/
│       └── 2026-02-Nightfall.md
├── 02_Vault/                    Finished, waiting for release
│   └── 2025-12-Horizons/
│       └── 2025-12-Horizons.md
└── 03_Catalog/                  Released songs with catalog numbers
    └── FW003-Lionheart/
        └── FW003-Lionheart.md
```

### Song Folder Convention

Each song lives in its own folder. The song file MUST be named exactly like its folder.

| Phase | Naming | Example |
|-------|--------|---------|
| `01_Projects/`, `02_Vault/` | `YYYY-MM-Songtitel/` | `2026-02-Nightfall/` |
| `03_Catalog/` | `PREFIX###-Songtitel/` | `FW004-Nightfall/` |

### Song File Format

```yaml
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

Frontmatter is the single source of truth. The Markdown body is for notes and generated release texts.

### Configuration

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

## Automatic Hooks

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows active songs and vault status at a glance |
| **Stop** | After each response | Reminds you to update the song `.md` file if lifecycle commands were used |

Hooks are advisory only — they inform, never block.

## Manual Workflow (No Skills)

music-production works without Claude Code:

1. Create folder: `01_Projects/2026-02-Mysong/`
2. Create file: `01_Projects/2026-02-Mysong/2026-02-Mysong.md` (file must be named exactly like its folder)
3. Add frontmatter: `status: production`, `created: 2026-02-20`
4. When done: move folder to `02_Vault/`, update `status: vault`
5. After release prep: move folder to `03_Catalog/`, rename to `FW004-Mysong/`

All files are standard Markdown — use any editor (Obsidian, VS Code, Finder, etc.).

## Documentation

- [CHANGELOG.md](CHANGELOG.md) - Version history
