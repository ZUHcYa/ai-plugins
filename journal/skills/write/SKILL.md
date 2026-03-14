---
name: "write"
description: "Add a journal entry — interactive or quick-write"
---

# /journal:write

## Purpose

Add entries to today's journal file. Auto-detects mode:
- **With args:** Quick-write — append directly, no prompts
- **Without args:** Interactive — ask for topic and content

## Usage

```
/journal:write                          Interactive mode
/journal:write Had a great meeting      Quick-write, no topic
/journal:write #standup API deployed     Quick-write with topic tag
```

---

## What to Do

1. Read `.journal/config.json`. If missing: `Run /journal:start first.` and stop.
2. Determine today's date and current time (HH:MM, local time).
3. Build filename: `YYYY-MM-DD.md` in `dataDir` from config.
4. If today's file does not exist, create it:
   ```markdown
   ---
   date: "YYYY-MM-DD"
   tags: []
   ---

   # YYYY-MM-DD
   ```
5. Determine mode:

### Quick-Write (args provided)

- If args start with `#word`: extract as topic tag, rest is content.
- Otherwise: entire args are content, no topic tag.
- No prompts, no confirmation dialog.

### Interactive (no args)

- Ask: `Topic tag? (optional, Enter to skip)`
- Ask: `What's on your mind?`
- No task prompts.

6. Format the entry:
   - With tag: `### HH:MM — Tag Title` (kebab-case to Title Case)
   - Without tag: `### HH:MM`
   - Blank line, then content.

7. Append to the day file (two blank lines before entry, one if file only has frontmatter + heading).

8. Update frontmatter `tags`: add new tag if not already present (lowercase kebab-case).

9. Output confirmation:
   ```
   Entry added -> <dataDir>/YYYY-MM-DD.md
   ```
   Then show the formatted entry.

---

## Tag Formatting

| Input | Heading | Stored in tags |
|-------|---------|----------------|
| `#deep-work` | `### HH:MM — Deep Work` | `deep-work` |
| `standup` (interactive) | `### HH:MM — Standup` | `standup` |
| (empty) | `### HH:MM` | *(none)* |

- Strip `#` prefix before display and storage
- Convert kebab-case to Title Case for heading
- Store in `tags` array as lowercase kebab-case

---

## Notes

- Always appends, never overwrites existing entries
- Timestamp is auto-generated — never ask for one
- Entries are counted by `### HH:MM` headings, not frontmatter
- Existing files with extra frontmatter fields (`entries`, `tasks_open`, etc.) are compatible — ignore them, don't remove them
