---
name: "check"
description: "Manual alignment check - are you still on track?"
---

# /compass:check

## Purpose

Deep alignment analysis of your current work against vision, session goal, and rules. Use this when you want a thorough review, not just the automatic hook checks.

## When to Use

- You feel like you might be drifting off course
- After a hook warning suggests running a check
- Before committing a large batch of changes
- When switching context within a session

## Workflow

```
User: /compass:check
Claude: Alignment Check

        Session Goal: "Add search functionality to plugin listing"
        Vision: "Build a marketplace for Claude Code plugins"

        Recent Work (git diff):
        - Modified: marketplace.json (added search index fields)
        - Created: src/search.ts (search implementation)
        - Created: src/utils/string-helpers.ts (fuzzy matching)
        - Modified: README.md (documented search feature)

        Assessment:
        | Dimension        | Status | Detail                                    |
        |------------------|--------|-------------------------------------------|
        | Vision-Alignment | OK     | Search enables plugin discovery            |
        | Session-Goal     | OK     | Directly implements the stated goal        |
        | YAGNI            | WARN   | string-helpers.ts has 5 unused functions   |
        | Scope Creep      | OK     | All changes relate to search               |
        | Custom Rules     | OK     | No new dependencies added                  |

        Recommendation:
        - Consider removing unused functions from string-helpers.ts
        - Otherwise on track. Keep going.
```

## What the Skill Does

1. Verify `.compass/` exists. If not: suggest `/compass:init`, stop
2. Read all state files:
   - `.compass/VISION.md` (vision, anti-goals, constraints, success criteria)
   - `.compass/session.md` (current session goal)
   - `.compass/rules.md` (custom rules)
3. Analyze current work:
   - Run `git diff` to see staged and unstaged changes
   - Run `git diff --name-only` to list changed files
   - Review recently created files
4. Evaluate across dimensions:
   - **Vision-Alignment:** Does the work support the overarching goal?
   - **Session-Goal:** Does the work match the session objective?
   - **Heuristic Check:** Apply YAGNI, Scope Creep, Over-Engineering, Gold Plating, Premature Optimization, NIH Syndrome
   - **Custom Rules:** Check against each active rule in rules.md
5. Output a compact assessment table with status per dimension
6. Provide concrete recommendations (not vague advice)
7. If goal needs adjustment: suggest a refined goal and offer to update session.md

## Assessment Status Values

| Status | Meaning |
|--------|---------|
| OK     | Aligned, no concerns |
| WARN   | Minor concern, worth noting |
| DRIFT  | Significant deviation, action recommended |

## Notes

- This skill reads but does NOT modify any files (unless user agrees to update session goal)
- Recommendations must be concrete and actionable, not generic
- If no git changes exist: analyze the conversation context instead
- If session.md is missing: skip session alignment, check only against vision
