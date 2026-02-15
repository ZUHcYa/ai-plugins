---
name: "focus"
description: "Define or change your session goal"
---

# /compass:focus

## Purpose

Sets the session goal that all automatic checks evaluate against. Can be called manually or triggered by the SessionStart hook reminder.

## When to Use

- At the start of a session (usually prompted by the hook)
- When your session goal changes mid-session
- To sharpen a vague goal into something concrete

## Workflow

### Without argument (interactive):

```
User: /compass:focus
Claude: Current vision: "Build a marketplace for Claude Code plugins"

        What do you want to accomplish this session?

User: Add search functionality to the plugin listing

Claude: Session goal set:
          "Add search functionality to the plugin listing"
          -> Supports vision: enables plugin discovery in marketplace

        .compass/session.md updated.
```

### With argument (quick mode):

```
User: /compass:focus "Fix the broken import in marketplace.json"
Claude: Session goal set:
          "Fix the broken import in marketplace.json"
          -> Supports vision: marketplace stability

        .compass/session.md updated.
```

### Goal doesn't align with vision:

```
User: /compass:focus "Add user authentication system"
Claude: Session goal set, but note:
          "Add user authentication system"
          -> Potential conflict with anti-goal: "no user accounts"

        Proceed anyway, or refine the goal?
```

## What the Skill Does

1. Check if `.compass/` exists. If not: suggest `/compass:init` first, stop
2. Read `.compass/VISION.md` for context (goal, anti-goals, constraints)
3. If argument provided: use it as session goal directly
4. If no argument: show 1-line vision summary, ask for session goal
5. Quick sanity check: does the session goal conflict with anti-goals or constraints?
   - If yes: warn (but don't block), ask user to confirm or refine
   - If no: proceed
6. Write `.compass/session.md` with:
   - The session goal
   - A 1-sentence link to how it supports the vision
   - Current date and time
7. Confirm the goal is set

## session.md Template

```markdown
---
date: <YYYY-MM-DD>
set-at: <HH:MM>
---

# Session-Ziel

<The concrete session goal>

## Bezug zur Vision

<1 sentence: How this goal supports the overarching vision>
```

## Notes

- Always overwrites the previous session goal (one goal at a time)
- The sanity check is advisory, not blocking - user decides
- If VISION.md is empty or missing sections, skip the sanity check
- Keep the interaction minimal: 1 question, 1 confirmation
