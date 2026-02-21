---
name: "journal"
description: "Add journal entries - interactive or one-shot"
---

# /journal

## Purpose

Add entries to today's journal file. Supports two modes:
- **Without args:** Interactive — Claude asks for topic and content
- **With args:** One-shot — directly append to today's file

## When to Use

- Writing a daily reflection or thought
- Logging a learning or insight
- Capturing a quick note with timestamp
- Adding tasks to today's journal (inline mode)

---

## Mode 1: Interactive (No Args)

```
User: /journal
Claude: Journal entry for 2026-02-21 (14:30)

        Topic tag? (optional, press Enter to skip)

User: deep-work
Claude: What's on your mind?

User: Spent 2 hours on the architecture document. Made good progress
      on the data model section. Key insight: keeping the entry format
      flat makes grep and manual editing much simpler.
Claude: Any tasks? (Enter tasks, one per line. Empty line to finish.)
        [Only shown when taskManagement is "inline"]

User: Review with team tomorrow
User: [empty line]
Claude: Entry added to entries/2026-02-21.md

        ### 14:30 — Deep Work

        Spent 2 hours on the architecture document...

        - [ ] Review with team tomorrow
```

### Interactive Without Topic

```
User: /journal
Claude: Journal entry for 2026-02-21 (18:00)

        Topic tag? (optional, press Enter to skip)

User: [Enter]
Claude: What's on your mind?

User: Quick reflection: productive day overall.
Claude: Entry added to entries/2026-02-21.md

        ### 18:00

        Quick reflection: productive day overall.
```

### Interactive With External Task System

When `taskManagement: "external"`, instead of asking for tasks:

```
Claude: Entry added to entries/2026-02-21.md

        ### 14:30 — Deep Work

        Spent 2 hours on the architecture document...

        Reminder: Track related tasks in Todoist.
```

---

## Mode 2: One-Shot (With Args)

```
User: /journal Quick standup note: blocked on API integration, waiting for credentials
Claude: Entry added to entries/2026-02-21.md

        ### 14:45

        Quick standup note: blocked on API integration, waiting for credentials
```

In one-shot mode:
- No topic tag by default (content goes directly as entry)
- No task prompt
- Timestamp is auto-generated
- Fast and minimal

### One-Shot With Topic Tag

Use `#tag` syntax at the start to set a topic:

```
User: /journal #standup Blocked on API integration, waiting for credentials
Claude: Entry added to entries/2026-02-21.md

        ### 14:45 — Standup

        Blocked on API integration, waiting for credentials
```

---

## What the Skill Does

1. Read `.journal/config.json`. If missing: error with hint to run `/journal:start`
2. Resolve `dataDir` from workspace root
3. Determine today's date and current time (HH:MM)
4. Build filename from `filePattern` (e.g. `2026-02-21.md`)
5. Check if today's file exists:
   - **If not:** Create new file with frontmatter and h1 heading
   - **If yes:** Read existing file for appending
6. Determine mode:
   - **No args:** Interactive mode — ask for topic tag, then content, then tasks (if inline)
   - **With args:** One-shot mode — parse optional `#tag` prefix, rest is content
7. Format entry:
   - `### HH:MM — Topic Tag` (if tag provided, capitalize first letter of each word)
   - Or `### HH:MM` (if no tag)
   - Empty line
   - Content (preserve user's formatting)
   - Empty line
   - Tasks as `- [ ] task` (only in interactive mode with inline task management)
8. Append entry to day file
9. Update frontmatter:
   - Increment `entries` count
   - Add topic tag to `tags` array (if not already present)
   - Update `tasks_open` count (if inline mode and tasks added)
10. Display confirmation with the formatted entry

---

## Day File Creation (New Day)

When no file exists for today, create it:

```markdown
---
date: "2026-02-21"
entries: 0
tags: []
tasks_open: 0
tasks_done: 0
created: "2026-02-21"
---

# 2026-02-21
```

Then immediately append the first entry (step 7-9 above).

If `taskManagement: "external"`, omit `tasks_open` and `tasks_done` from frontmatter.

---

## Topic Tag Formatting

| User input | Formatted heading |
|------------|-------------------|
| `deep-work` | `### HH:MM — Deep Work` |
| `#standup` | `### HH:MM — Standup` |
| `project alpha` | `### HH:MM — Project Alpha` |
| (empty) | `### HH:MM` |

Rules:
- Convert kebab-case to Title Case for display in heading
- Store original form (lowercase, kebab-case) in frontmatter `tags` array
- `#` prefix is stripped (it's just a one-shot convenience syntax)

---

## Task Checkbox Updates

When the user manually checks a task in the day file (changes `- [ ]` to `- [x]`),
the frontmatter counters (`tasks_open`, `tasks_done`) may be out of sync.

**This is acceptable.** The frontmatter counters are convenience fields for the overview.
They are updated by the skill on entry creation, but may drift if the user edits manually.

---

## External Task Reference

When `taskManagement: "external"` and `taskSystemRef` is set:
- Do NOT add `- [ ]` checkboxes to entries
- After adding an entry in interactive mode, show: `Reminder: Track related tasks in <taskSystemRef>.`
- In the day file, do NOT include `tasks_open`/`tasks_done` in frontmatter

---

## Input Validation

| Field | Validation |
|-------|-----------|
| Content | Non-empty string |
| Topic tag | Optional, alphanumeric + hyphens, max 30 chars |
| Tasks | Optional, non-empty strings |

---

## Notes

- Two modes: interactive (conversational) and one-shot (fast)
- Always appends, never overwrites existing entries
- Timestamp is auto-generated from current time
- Topic tags are optional — not every thought needs a category
- YAML frontmatter is updated automatically but can be maintained manually
- Works without config for manual use — just create YYYY-MM-DD.md files with the format above
