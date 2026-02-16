---
name: "init"
description: "Initialize compass vision tracking for the current project"
---

# /compass:init

## Purpose

Sets up the `.compass/` directory in your project root with vision, rules, and session files. Guides you through defining your project vision interactively.

## When to Use

- First time using compass in a project
- When no `.compass/` directory exists
- To rebuild the vision structure from scratch

## Workflow

### Fresh setup:

```
User: /compass:init
Claude: No .compass/ directory found. Let's set up vision tracking.

        First question: What is the overarching goal of this project?

User: Build a marketplace for Claude Code plugins

Claude: Got it. Now: What should this project explicitly NOT do?
        (Anti-goals help prevent scope creep)

User: No paid features, no user accounts, no hosting infrastructure

Claude: Last question: What constraints apply?
        (Time, tech stack, scope limits)

User: Pure markdown plugins, no compiled code, ship MVP in 2 weeks

Claude: Created .compass/ structure:
          VISION.md     (project vision with goals, anti-goals, constraints)
          rules.md      (default rules + custom slot)
          session.md    (empty - will be set at session start)

        Vision tracking active!
        The SessionStart hook will ask for your session goal automatically.
        Use /compass:check anytime for a manual alignment review.
```

### Already exists:

```
User: /compass:init
Claude: compass (v0.1.0) - .compass/ already exists:
          VISION.md     (last updated: 2025-01-15)
          rules.md      (3 active rules)
          session.md    (goal: "Add search feature")

        Overwrite and start fresh? This will reset your vision.
        Use /compass:check instead to review current alignment.
```

## What the Skill Does

1. Read version from `compass/.claude-plugin/plugin.json` (= installed version)
2. Version-Check: WebFetch `https://raw.githubusercontent.com/ZUHcYa/ai-plugins/main/compass/.claude-plugin/plugin.json`, JSON parsen, `version` Feld extrahieren (= latest version).
   - Wenn latest > installed: Update-Hinweis merken fuer Ausgabe
   - Wenn gleich oder Fetch fehlschlaegt: kein Hinweis
3. Check if `.compass/` directory exists in the current working directory
4. If it already exists:
   a. Display version in output header (mit Update-Hinweis falls vorhanden: `compass (v0.1.0 - Update verfuegbar: v0.2.0)`)
   b. Report current state (files, last updated, active rules count)
   c. Ask if user wants to reset (destructive - requires confirmation)
   d. If no reset: suggest `/compass:check` instead
5. If it does not exist:
   a. Ask three questions interactively:
      - **Goal:** "What is the overarching goal of this project?"
      - **Anti-Goals:** "What should this project explicitly NOT do?"
      - **Constraints:** "What constraints apply? (time, tech, scope)"
   b. Create `.compass/` directory
   c. Create `VISION.md` from template, populated with user answers
   d. Create `rules.md` with default rules
   e. Create empty `session.md` placeholder
6. Confirm setup and remind about SessionStart hook

## VISION.md Template

```markdown
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
---

# Vision

## Ziel

<User's answer to the goal question>

## Anti-Ziele

<User's answer to the anti-goals question - as bullet list>

## Constraints

<User's answer to the constraints question - as bullet list>

## Erfolgskriterien

<Derived from goal - 2-3 concrete, measurable criteria>
```

## rules.md Template

```markdown
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
---

# Custom Rules

Project-specific rules checked by compass on every alignment evaluation.
Edit this file directly to add your own rules.

## Active Rules

- No new dependencies without clear justification
- No code for hypothetical future requirements
- No abstraction until the pattern repeats 3+ times

## Deactivated Rules

<!-- Move rules here that should be temporarily ignored -->
```

## session.md Placeholder

```markdown
---
date: <YYYY-MM-DD>
set-at: pending
---

# Session-Ziel

Not yet defined. The SessionStart hook will prompt you, or use /compass:focus.
```

## Notes

- `.compass/` lives in the project root, NOT in the plugin directory
- Files are plain markdown - editable in any editor, git-trackable
- The user can (and should) edit VISION.md and rules.md directly as the project evolves
- Erfolgskriterien are derived by Claude from the stated goal, not asked separately
- All templates use German section headers (consistent with project convention)

## Output Guidelines

- Keep setup interaction concise: 3 questions, no more
- Confirm what was created with file list
- Remind about automatic hooks and manual skills
