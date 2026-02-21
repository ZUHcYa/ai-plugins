# journal Quick-Start Guide

Get started with journal in 5 minutes.

---

## 1. Setup

```
/journal:start
```

On first run, journal asks for your preferences:

```
User: /journal:start
Claude: Welcome to journal!

        Where should entries be stored? [Default: ./entries]
User: ./entries

Claude: Filename pattern?
        - YYYY-MM-DD (default)
        - YYYY-MM-DD-ddd (with weekday)
User: YYYY-MM-DD

Claude: Do you use an existing task management system? [Y/n]
User: N

Claude: Created: .journal/config.json
        Created: entries/

        Ready! Use /journal to add your first entry.
```

---

## 2. Add a Journal Entry

### Interactive Mode

```
/journal
```

Claude asks for your topic and content:

```
User: /journal
Claude: Journal entry for 2026-02-21 (14:30)

        Topic tag? (optional, Enter to skip)
User: deep-work

Claude: What's on your mind?
User: Made great progress on the architecture doc today.

Claude: Entry added to entries/2026-02-21.md

        ### 14:30 — Deep Work

        Made great progress on the architecture doc today.
```

### One-Shot Mode

```
/journal Quick thought: simplicity wins over flexibility every time
```

Appends instantly, no questions asked.

---

## 3. Check Your Status

```
/journal:start
```

Run anytime to see recent entries and statistics:

```
User: /journal:start
Claude: Journal Overview
        ======================================

        This Week (3 entries)
        ----------------------
        2026-02-21  3 entries  [deep-work, project-alpha]
        2026-02-20  2 entries  [reading]
        2026-02-19  1 entry   [planning]

        Statistics
        ----------
        Total: 18 entries across 12 days this month
        Streak: 3 days

        Actions
        -------
        /journal to add today's next entry
```

---

## The Complete Workflow

```
/journal:start -> /journal -> /journal:start
     |               |             |
   Setup          Add entry    Review status
```

---

## Skills

| Skill | When to use |
|-------|-------------|
| `/journal:start` | Setup (first run) or overview (after setup) |
| `/journal` | Add a journal entry (interactive or one-shot) |

---

## Manual Workflow (No Skills)

journal works without Claude Code skills too:

1. Create folder: `entries/`
2. Create file: `entries/2026-02-21.md`
3. Add frontmatter: `date`, `entries`, `tags`
4. Add entries with `### HH:MM — Topic` heading pattern
5. Use `- [ ] task` for inline tasks

All files are standard Markdown — use any editor (Obsidian, VS Code, Notion, etc.).
