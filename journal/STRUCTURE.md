# journal Folder Structure

Overview of how journal files are organized on disk.

---

## Path Structure

journal uses a simple flat folder structure:

```
<dataDir>/
├── 2026-02-21.md
├── 2026-02-20.md
├── 2026-02-19.md
└── ...
```

**Example with `dataDir: "./entries"`:**

```
entries/
├── 2026-02-21.md
├── 2026-02-20.md
└── 2026-02-19.md
```

---

## Why This Structure

| Decision | Reason |
|----------|--------|
| Flat folder (no year/month subfolders) | Simple, greppable, works in any file browser |
| One file per day | Natural boundary, easy to find entries by date |
| Date as filename | Chronological sorting by default in any file manager |

---

## File Naming

| Pattern | Example |
|---------|---------|
| `YYYY-MM-DD.md` (default) | `2026-02-21.md` |
| `YYYY-MM-DD-ddd.md` (optional) | `2026-02-21-fri.md` |

Configurable via `filePattern` in `.journal/config.json`.

---

## Day File Format

```yaml
---
date: "2026-02-21"
entries: 2
tags: ["deep-work", "reflection"]
tasks_open: 1
tasks_done: 3
created: "2026-02-21"
---

# 2026-02-21

### 14:30 — Deep Work

Entry content here...

- [x] Completed task
- [ ] Open task

### 18:00

Quick reflection without a topic tag.
```

**YAML frontmatter is the single source of truth** for structured metadata.
The Markdown body contains the actual journal content.

---

## Required vs. Optional Fields (Frontmatter)

| Field | Required | Notes |
|-------|----------|-------|
| `date` | Yes | YYYY-MM-DD |
| `entries` | Auto | Count of entries in this file |
| `tags` | Auto | Collected topic tags |
| `tasks_open` | Auto | Only when taskManagement is "inline" |
| `tasks_done` | Auto | Only when taskManagement is "inline" |
| `created` | Auto | Set on file creation |

---

## Entry Format (Within Day File)

Each entry follows this pattern:

```markdown
### HH:MM — Topic Tag

Content here. Free-form text.

- [ ] Optional task (inline mode only)
```

- `### HH:MM` is the minimum (topic tag is optional)
- Entries are appended chronologically (newest at bottom)
- Tasks use standard Markdown checkboxes

---

## Configuration

**Location:** `.journal/config.json`

```json
{
  "dataDir": "./entries",
  "filePattern": "YYYY-MM-DD",
  "taskManagement": "inline",
  "taskSystemRef": null
}
```

---

**Version:** 0.1.0
