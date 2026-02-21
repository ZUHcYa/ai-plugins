# journal - Personal Journal

A daily journal for reflections, thoughts, and learnings.

## What It Does

- **Writing** timestamped entries with optional topic tags
- **Tracking** your journaling streak and habits
- **Managing** tasks inline or referencing your existing task system
- **Reviewing** recent entries and statistics at a glance

## Quick Start

```
/journal:start        Setup on first run, overview afterwards
/journal              Add a journal entry (interactive or one-shot)
```

## Skills

| Skill | Purpose |
|-------|---------|
| `/journal:start` | Smart entry point - setup on first run, overview afterwards |
| `/journal` | Add journal entries - interactive or one-shot mode |

## State

Config lives in `.journal/config.json`. Journal entries are stored as individual Markdown files
(one per day) with YAML frontmatter — readable in any editor, no Claude Code required.

```
entries/
  2026-02-21.md
  2026-02-20.md
  2026-02-19.md
```

File format: `YYYY-MM-DD.md` with timestamped entries inside.

## Automatic Hooks

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows today's entry count or last entry date |

The hook is advisory only — it informs, never blocks.

## Documentation

- [QUICKSTART.md](QUICKSTART.md) - 5-minute getting started guide
- [STRUCTURE.md](STRUCTURE.md) - Folder structure and file format reference
- [CHANGELOG.md](CHANGELOG.md) - Version history
