---
description: "Quick journal entry — write directly without interaction"
allowed_tools:
  - Read
  - Write
  - Edit
---

# /journal

Add an entry to today's journal file. No questions, no confirmation — just write.

## Usage

```
/journal <text>
/journal #tag <text>
```

- `/journal Today was productive` — plain entry, no topic
- `/journal #standup API deployment done` — entry with topic tag "Standup"

If invoked without text: output `Usage: /journal <text> | /journal #tag <text>` and stop.

---

## What to Do

1. Read `.journal/config.json`. If missing: output `Error: journal not set up. Run /journal:start first.` and stop.

2. Determine today's date and current time (HH:MM, local time).

3. Build the filename: apply `filePattern` to today's date (default: `YYYY-MM-DD`) → e.g. `entries/2026-02-27.md` using the `dataDir` from config.

4. If today's file does not exist: create it with this exact content (no trailing blank line after frontmatter block):
   ```markdown
   ---
   date: "YYYY-MM-DD"
   entries: 0
   tags: []
   tasks_open: 0
   tasks_done: 0
   created: "YYYY-MM-DD"
   ---

   # YYYY-MM-DD
   ```
   Omit `tasks_open` and `tasks_done` if `taskManagement` is `"external"`.

5. Parse the input text:
   - If it starts with `#word` (a `#` immediately followed by non-space characters): extract as topic tag, rest is content.
   - Otherwise: entire text is content, no topic tag.

6. Format the entry:
   - With tag: `### HH:MM — Tag Title` (convert kebab-case to Title Case, strip `#`)
   - Without tag: `### HH:MM`
   - Followed by a blank line, then the content text.

7. Append to the day file: two blank lines before the new entry block (or one blank line if the file only has the frontmatter + heading so far).

8. Update frontmatter:
   - Increment `entries` by 1.
   - If a topic tag was used and it is not already in the `tags` array: append it (store lowercase, kebab-case form).

9. Output a single confirmation line:
   ```
   Entry added → entries/YYYY-MM-DD.md
   ```
   Then show the formatted entry block that was written.

---

## Tag Formatting Rules

| Input | Heading | Stored in tags |
|-------|---------|----------------|
| `#standup blocked` | `### HH:MM — Standup` | `standup` |
| `#deep-work insight` | `### HH:MM — Deep Work` | `deep-work` |
| `just a thought` | `### HH:MM` | *(none)* |

Rules:
- Strip `#` prefix before display and storage.
- Convert kebab-case to Title Case for the heading (each word capitalized).
- Store in `tags` array as lowercase kebab-case.

---

## Notes

- Always appends, never overwrites existing entries.
- Timestamp is auto-generated — do not ask the user to provide one.
- Do not ask for tasks, topic confirmation, or anything else.
- If `taskManagement` is `"external"`, do not add checkboxes or task prompts.
