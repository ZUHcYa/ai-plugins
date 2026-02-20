# music-production - Music Production Workflow Companion

A lifecycle companion for music production that guides songs from idea to catalog.

## What It Does

**music-production** manages your song lifecycle through structured phases:

- **Creating** new song projects with consistent templates
- **Moving** finished productions to vault for release preparation
- **Enriching** songs with metadata, release texts, and cover art prompts
- **Archiving** released songs to your catalog with sequential numbering

## Quick Start

```
/music-production:start       Setup on first run, overview afterwards
/music-production:new         Create a new song project
/music-production:vault       Move finished song to vault
/music-production:release     Prepare song for release (metadata + sections)
/music-production:generate    Generate release texts and cover art prompt
/music-production:catalog     Archive to catalog with catalog number
```

## Phase Lifecycle

```
01_Projects/     ->  02_Vault/      ->  Release-Prep     ->  03_Catalog/
Production           Finished           Texts & Meta          Published

/mp:new              /mp:vault          /mp:release           /mp:catalog
                                        /mp:generate
```

## Skills

| Skill | Purpose |
|-------|---------|
| `/music-production:start` | Smart entry point - setup on first run, overview afterwards |
| `/music-production:new` | Create a new song project in `01_Projects/` |
| `/music-production:vault` | Move finished song from `01_Projects/` to `02_Vault/` |
| `/music-production:release` | Enrich song file with metadata (BPM, Key, Mood) and release sections |
| `/music-production:generate` | Generate YouTube/Bandcamp descriptions and cover art prompt |
| `/music-production:catalog` | Move released song to `03_Catalog/` with catalog number (e.g. FW004) |

## Folder Structure

The plugin expects this structure to already exist in your music production root:

```
Music-Root/
  01_Projects/          Active productions
  02_Vault/             Finished, waiting for release
  03_Catalog/           Released and archived
  .music-production/    Plugin config (created by /music-production:start)
```

Each song lives in its own folder with a central `<Ordnername>.md` file (named exactly like its folder).

## State

Config lives in `.music-production/config.json` (created automatically on first run).

Files are plain markdown with YAML frontmatter - readable in any editor, no Claude Code required.

## Automatic Hooks

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows active songs and vault status at a glance |
| **Stop** | After each response | Reminds you to update the song `.md` file if lifecycle commands were used |

Hooks are advisory only — they inform, never block.

## Documentation

- [QUICKSTART.md](QUICKSTART.md) - 5-minute getting started guide
- [STRUCTURE.md](STRUCTURE.md) - Folder structure and file format reference
- [CHANGELOG.md](CHANGELOG.md) - Version history
