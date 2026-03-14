# journal - Personal Journal

A daily journal for reflections, thoughts, and learnings.

## What It Does

- **Writing** timestamped entries with optional topic tags
- **Tracking** your journaling streak and habits
- **Reviewing** recent entries and statistics at a glance

## Quick Start

```
/journal:start                          Setup on first run, overview afterwards
/journal:write                          Interactive entry (asks for topic + content)
/journal:write Had a great meeting      Quick-write without prompts
/journal:write #standup API deployed    Quick-write with topic tag
```

## Skills

| Skill | Purpose |
|-------|---------|
| `/journal:start` | Setup (first run) or overview (after setup) |
| `/journal:write` | Add entries — interactive or quick-write |

## File Structure

Config lives in `.journal/config.json` (single field: `dataDir`).
Journal entries are stored as individual Markdown files — one per day, flat folder:

```
entries/
  2026-02-21.md
  2026-02-20.md
  2026-02-19.md
```

### Day File Format

```yaml
---
date: "2026-02-21"
tags: ["deep-work", "reflection"]
---

# 2026-02-21

### 14:30 — Deep Work

Entry content here...

### 18:00

Quick reflection without a topic tag.
```

- Frontmatter: `date` (required), `tags` (auto-collected)
- Entries: `### HH:MM — Topic Tag` headings, appended chronologically
- Filename: always `YYYY-MM-DD.md`

## Manual Workflow (No Skills)

journal works without Claude Code:

1. Create folder: `entries/`
2. Create file: `entries/2026-02-21.md`
3. Add frontmatter: `date`, `tags`
4. Add entries with `### HH:MM — Topic` heading pattern

All files are standard Markdown — use any editor (Obsidian, VS Code, Notion, etc.).

## Automatic Hooks

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows today's entry count or last entry date |

The hook is advisory only — it informs, never blocks.

## Documentation

- [CHANGELOG.md](CHANGELOG.md) - Version history
