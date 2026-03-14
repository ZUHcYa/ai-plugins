---
name: "start"
description: "Smart entry point - setup on first run, overview afterwards"
---

# /journal:start

## Purpose

Single entry point for journal. Automatically detects context:
- **First run:** Sets up config and data directory
- **After setup:** Shows overview of recent entries and statistics

---

## First Run (No Config)

When no `.journal/config.json` exists:

```
User: /journal:start
Claude: Welcome to journal!

        Where should journal entries be stored?
        [Default: ./entries]

User: ./entries
Claude: Created: .journal/config.json
        Created: entries/

        Ready! Use /journal:write to add your first entry.
```

### What to Do (Setup)

1. Check if `.journal/config.json` exists
2. If NOT:
   a. Ask for `dataDir` (default: `./entries`) — this is the only setup question
   b. Write `.journal/config.json`:
      ```json
      {
        "dataDir": "./entries"
      }
      ```
   c. Create `dataDir` folder if it does not exist

---

## After Setup (Config Exists)

Shows overview of recent entries and statistics:

```
User: /journal:start
Claude: Journal Overview
        ======================================

        This Week
        ----------------------
        2026-02-21  3 entries  [deep-work, project-alpha]
        2026-02-20  2 entries  [reading, reflection]
        2026-02-19  1 entry   [planning]

        Statistics
        ----------
        This month: 18 entries across 12 days
        Current streak: 3 days
        Top tags: deep-work (5), reflection (4), planning (3)

        Actions
        -------
        /journal:write to add today's entry
```

### What to Do (Overview)

1. Read `.journal/config.json`
2. Resolve `dataDir` from workspace root
3. Scan all `.md` files in `dataDir` matching `YYYY-MM-DD.md`
4. For the last 7 days: list day files with entry count and tags
   - **Count entries by `### HH:MM` headings** in the file body (not frontmatter)
   - Read `tags` from frontmatter
5. Calculate statistics:
   - Total entries this month (count `### HH:MM` headings across all files)
   - Days with entries this month
   - Current streak (consecutive days with entries, counting back from today)
   - Most used tags (top 3-5 by frequency)
6. Display overview with suggested actions

---

## Suggested Actions

| Condition | Suggestion |
|-----------|------------|
| No entry today | "/journal:write to add today's entry" |
| Entry exists today | "Last entry: YYYY-MM-DD at HH:MM" |
| No entries at all | "Use /journal:write to write your first entry!" |

---

## Notes

- Single config field: `dataDir` — nothing else to configure
- Filename pattern is always `YYYY-MM-DD.md` (hardcoded, no config needed)
- Entries are counted by `### HH:MM` headings, not frontmatter fields
- Existing files with extra frontmatter fields are compatible — ignore them
