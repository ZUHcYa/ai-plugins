---
name: "new"
description: "Create a new song project in 01_Projects/"
---

# /music-production:new

## Purpose

Creates a new song project folder in `01_Projects/` with a song file (`<FolderName>.md`) from the canonical template.

## When to Use

- Starting work on a new song
- Capturing an initial idea with reference tracks

## Workflow

```
User: /music-production:new
Claude: What's the song title?

User: Nightfall
Claude: Any reference tracks? (optional, skip with Enter)

User: Radiohead - Everything In Its Right Place - floating atmosphere
Claude: Created:
        01_Projects/2026-02-Nightfall/
        ├── 2026-02-Nightfall.md
        ├── 01_RAW/
        ├── 02_AUDIO/
        ├── 03_VIDEO/
        ├── 04_ARTWORK/
        └── 05_OUTPUT/

        Status: production
        Reference: 1 track added

        Open 2026-02-Nightfall.md to add more notes and TODOs as you produce.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists. If not: inform the user to run `/music-production:start` first and stop.
2. Check that `01_Projects/` exists. If the folder is missing, the workspace is not set up — prompt the user to run `/music-production:start` first and stop.
3. Ask for the song title.
4. Derive the folder name: `YYYY-MM-<Title>` using the current date and title-case, hyphens for spaces.
5. Check whether `01_Projects/<FolderName>/` already exists.
   - If yes: warn the user ("A folder with that name already exists") and offer two options:
     a. Enter a different title
     b. Add a numeric suffix (e.g. `2026-02-Nightfall-2`)
6. Optionally ask for initial reference track(s) with notes.
7. Create the folder in `01_Projects/`.
8. Create five sub-folders inside the song folder:
   - `01_RAW/` - raw material (recordings, samples, field recordings)
   - `02_AUDIO/` - audio files (bounces, stems, mixdowns)
   - `03_VIDEO/` - video content
   - `04_ARTWORK/` - cover art and visual assets
   - `05_OUTPUT/` - final files (master, release formats)
9. Create the song file with **exactly the same name as the folder** + `.md`.
   - Folder `2026-02-Nightfall` -> file `2026-02-Nightfall.md`
   - Folder `2026-01-Lost-In-Time` -> file `2026-01-Lost-In-Time.md`
10. Fill in title, date, and optional references from step 6.

> **CRITICAL - File naming:** The song file ALWAYS carries exactly the folder name + `.md`. Example: folder `2026-02-Nightfall/` -> file `2026-02-Nightfall.md`. NEVER use `_song.md`, `song.md`, or any other generic name.

## Naming Convention

| Part | Format | Example |
|------|--------|---------|
| Date prefix | `YYYY-MM` | `2026-02` |
| Separator | `-` | `-` |
| Title | Title-Case, hyphens for spaces | `Nightfall`, `Lost-In-Time` |
| Full | Combined | `2026-02-Nightfall` |

**Rules:**
- Spaces are replaced with hyphens
- Title case for readability
- Umlauts and accented characters are allowed in the folder name (e.g. `2026-02-Nachtfall` or `2026-02-Über-Alles`). Most operating systems and Markdown editors handle these without issue. If a platform has restrictions, the user can manually rename after creation.
- Avoid other special characters (slashes, colons, asterisks, question marks)

## Edge Cases

### Name Collision

If `01_Projects/2026-02-Nightfall/` already exists:

```
Claude: A folder named "2026-02-Nightfall" already exists in 01_Projects/.

        Choose an option:
        [1] Enter a different title
        [2] Use "2026-02-Nightfall-2" as suffix

User: 1
Claude: What's the new title?
```

Never overwrite or merge with an existing folder silently.

### Missing `01_Projects/` Folder

If `01_Projects/` does not exist in the current directory:

```
Claude: Cannot find the "01_Projects/" folder.

        This usually means you are not in your Music-Production root folder,
        or the workspace has not been set up yet.

        Run /music-production:start to initialize the workspace.
```

The skill does not create `01_Projects/` itself. The folder structure must be in place before songs can be created.

### Missing Config

If `.music-production/config.json` does not exist:

```
Claude: No music-production config found.

        Run /music-production:start to set up the workspace first.
```

### Special Characters in Title

Umlauts (ä, ö, ü, ß) and accented characters are kept as-is in folder and file names. They work reliably on macOS, Linux, and Windows, and are readable in all standard Markdown editors.

Example: title "Über den Wolken" becomes folder `2026-02-Über-Den-Wolken/` and file `2026-02-Über-Den-Wolken.md`.

If the user's environment cannot handle non-ASCII folder names, they can enter an ASCII-equivalent title instead.

---

## Song Template

Canonical template for new songs in the production phase:

```markdown
---
tags:
  - song
type: song
genre:
status: production
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# [Song Title]

## Referenzen
*Verlinke Tracks mit kurzer Notiz, was du davon willst:*

---

## Notizen
*Ideen, Sounds, Konzept - was auch immer*

## TODOs
- [ ]
```

### Template Field Reference

| Field | Set by Skill | User-editable |
|-------|-------------|---------------|
| `tags` | `song` (default) | Yes - add more tags |
| `type` | `song` (fixed) | No |
| `genre` | Empty | Yes - fill during production |
| `status` | `production` | No - managed by skills |
| `created` | Current date | No |
| `updated` | Current date | Auto-updated by skills |

---

## Notes

- Requires config (run `/music-production:start` first)
- Creates folder, 5 sub-folders, and song file in one step
- Song file is ALWAYS named the same as the folder (e.g. folder `2026-02-Nightfall/` -> file `2026-02-Nightfall.md`)
- Template is intentionally minimal - it grows during production
- References use `[[wiki-link]]` format for Obsidian compatibility (works as plain text in other editors)
