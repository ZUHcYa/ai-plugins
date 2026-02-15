---
name: "retro"
description: "Session retrospective - what was achieved vs. planned"
---

# /compass:retro

## Purpose

Reviews the session: compares what was planned against what was actually done. Identifies drift moments, captures learnings, and optionally evolves the vision.

## When to Use

- At the end of a session
- When a Stop hook suggests significant drift
- After completing a major milestone
- When you want to reflect before moving on

## Workflow

```
User: /compass:retro
Claude: Session Retrospective

        Session Goal: "Add search functionality to plugin listing"
        Duration: ~2 hours (based on git timestamps)

        Achieved (aligned with goal):
        - Implemented search index in marketplace.json
        - Created search component with fuzzy matching
        - Added search tests (3/3 passing)

        Drift (outside session goal):
        - Refactored plugin loader (not related to search)
        - Added dark mode toggle to settings (unplanned feature)

        Drift Analysis:
        - Plugin loader refactor started as "needed for search" but
          expanded beyond search requirements
        - Dark mode was a tangent triggered by noticing a UI issue

        Learnings:
        - Refactoring that "enables" a feature tends to expand beyond need
        - UI tangents are a recurring drift pattern

        Vision Update Needed? No - vision still accurate.
        Rules Update? Consider adding: "No UI changes outside session scope"

        Update rules.md with this new rule? [y/n]
```

## What the Skill Does

1. Verify `.compass/` exists. If not: suggest `/compass:init`, stop
2. Read all state files (VISION.md, session.md, rules.md)
3. Analyze session work:
   - `git log` since session start (use session.md timestamp)
   - `git diff` for uncommitted changes
   - List all files created/modified
4. Categorize each change:
   - **Achieved:** Directly supports the session goal
   - **Drift:** Does not directly support the session goal
5. For drift items: identify the moment and reason for the deviation
6. Extract learnings:
   - Recurring patterns in drift behavior
   - What worked well for staying focused
   - What triggered the deviations
7. Assess if vision needs updating:
   - Did the vision evolve during the session?
   - Are there new anti-goals or constraints discovered?
8. Assess if rules need updating:
   - Did the session reveal new anti-patterns?
   - Should any existing rules be refined?
9. Offer to update VISION.md and/or rules.md (with user confirmation)

## Notes

- Only modifies files if user explicitly confirms
- The retro is factual, not judgmental - drift isn't failure, it's data
- If no session.md exists: do a lightweight retro based on recent git history
- If no git changes: base the retro on conversation context
- Keep the output scannable: use lists and tables, not paragraphs
