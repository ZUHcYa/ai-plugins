---
name: "start"
description: "Smart entry point - setup on first run, overview afterwards"
---

# /journal:start

## Purpose

Single entry point for journal. Automatically detects context:
- **First run:** Sets up config and data directory
- **After setup:** Shows overview of recent entries and statistics

## When to Use

- Starting with journal for the first time
- Beginning a session - get oriented quickly
- After a break - see recent entries at a glance
- Quick status check

---

## First Run (No Config)

When no `.journal/config.json` exists, setup runs automatically:

```
User: /journal:start
Claude: Welcome to journal!

        Where should journal entries be stored?
        (Relative path, e.g. "./entries" or "Documents/Journal")
        [Default: ./entries]

User: ./entries
Claude: Filename pattern?
        - YYYY-MM-DD (default) -> 2026-02-21.md
        - YYYY-MM-DD-ddd -> 2026-02-21-fri.md

User: YYYY-MM-DD
Claude: Task management?
        Do you use an existing task management system (Todoist, Jira, etc.)?

        [Y] Yes - I'll reference it in entries
        [N] No - use inline Markdown checkboxes

User: N
Claude: Setting up...

        Created: .journal/config.json
        Created: entries/

        Ready! Use /journal to add your first entry.
```

### With External Task System

```
User: Y
Claude: What's your task system? (e.g. "Todoist", "Jira", "Obsidian Tasks")

User: Todoist
Claude: Setting up...

        Created: .journal/config.json (tasks -> Todoist reference)
        Created: entries/

        Ready! Use /journal to add your first entry.
        Task references will link to Todoist.
```

---

## After Setup (Config Exists)

When config exists, shows overview of recent entries and statistics:

```
User: /journal:start
Claude: Journal Overview
        ======================================

        This Week (3 entries)
        ----------------------
        2026-02-21  3 entries  [deep-work, project-alpha]
        2026-02-20  2 entries  [reading, reflection]
        2026-02-19  1 entry   [planning]

        Statistics
        ----------
        Total entries this month: 18 entries across 12 days
        Current streak: 3 days
        Most used tags: deep-work (5), reflection (4), planning (3)

        Tasks (inline)
        ---------------
        Open: 4 | Done this week: 11

        Actions
        -------
        1. /journal to add today's entry
        2. Last entry: 2026-02-21 at 18:00
```

### Empty Journal

```
User: /journal:start
Claude: Journal Overview
        ======================================

        No entries yet.

        Get Started
        -----------
        Use /journal to write your first entry!
```

### With External Task System

When `taskManagement: "external"`, the Tasks section shows:

```
        Tasks -> Todoist
        -----------------
        (Managed externally)
```

---

## What the Skill Does

### First Run (Setup)

1. Check if `.journal/config.json` exists
2. If NOT:
   a. Ask for `dataDir` (default: `./entries`)
   b. Ask for `filePattern` (default: `YYYY-MM-DD`)
   c. Ask about task management preference
      - If external: ask for system name, store as `taskSystemRef`
      - If inline: store `taskManagement: "inline"`
   d. Write `.journal/config.json`
   e. Create `dataDir` folder if it does not exist

### After Setup (Overview)

1. Read `.journal/config.json`
2. Read version from `journal/.claude-plugin/plugin.json` for display in the status header
3. Resolve `dataDir` from workspace root
4. Scan all `.md` files in `dataDir` matching the filename pattern
5. For the last 7 days: list day files with entry count and tags (from frontmatter)
6. Calculate statistics:
   - Total entries this month (sum of `entries` from frontmatter)
   - Days with entries this month
   - Current streak (consecutive days with entries, counting backwards from today)
   - Most used tags (top 3-5 by frequency this month)
7. If `taskManagement: "inline"`: sum `tasks_open` and `tasks_done` from this week's files
8. Display overview with suggested actions

---

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| No entry today | "/journal to add today's entry" |
| Entry exists today | "Last entry: YYYY-MM-DD at HH:MM" |
| Streak > 0 | "Current streak: N days" |
| Open tasks > 5 (inline mode) | "N open tasks - review and close completed ones" |
| No entries at all | "Use /journal to write your first entry!" |

---

## Configuration File

**Location:** `.journal/config.json`

```json
{
  "dataDir": "./entries",
  "filePattern": "YYYY-MM-DD",
  "taskManagement": "inline",
  "taskSystemRef": null
}
```

| Field | Purpose |
|-------|---------|
| `dataDir` | Where day files are stored (relative to workspace root) |
| `filePattern` | Filename pattern for day files |
| `taskManagement` | `"inline"` for checkboxes, `"external"` for reference |
| `taskSystemRef` | Name of external task system (`null` if inline) |

---

## Notes

- Single command to remember: just `/journal:start`
- Context-aware: detects first run vs. returning user
- Plugin creates data directory if needed
- Actionable suggestions help users know what to do next
- Statistics are calculated from YAML frontmatter (fast, no content parsing needed)
