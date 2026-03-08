---
name: "start"
description: "Smart research entry point - setup on first run, overview afterwards"
---

# /research:start

## Purpose

Single entry point for the research plugin. Automatically detects context:
- **First run:** Sets up folder structure and config
- **After setup:** Shows research report overview

## When to Use

- Starting with the research plugin for the first time
- Beginning a session - see all research reports at a glance
- Quick status check - which reports need attention?

---

## First Run (No Config)

When no `.research/config.json` exists, setup runs automatically.

```
User: /research:start
Claude: Welcome to the Research Plugin!

        Install here (./)? [Y/n]

User: Y
Claude: Setting up your research workspace...

        Created:
        ├── research/
        └── .research/config.json

        Ready! Place research reports in research/ to get started.
```

---

## After Setup (Config Exists)

```
User: /research:start
Claude: Research Status
        ======================================

        Reports (4)
        -----------
        🟢 Machine Customers Analysis [VERIFIED]
        🟢 AI Commerce Trends [VERIFIED]
        🟡 Fitness Market Study [DRAFT]
        🟡 Vendor Analysis [DRAFT] — audit available → /research:finalize

        Suggested Actions
        -----------------
        1. Vendor Analysis has audit ready → /research:finalize
        2. Fitness Market Study is draft > 14 days → /research:evaluate or audit
        3. Start new research → /research:investigate <topic>
        4. Answer a focused question → /research:review "question"
```

## What the Skill Does

### First Run (Setup)

1. Check if `.research/config.json` exists
2. If NOT: ask for target path (default: `./`)
3. Write `.research/config.json` with `targetPath`
4. Create `research/` folder at targetPath

### After Setup (Overview)

1. Read `.research/config.json`
2. Scan `research/` folder for .md files (excluding `-audit.md` files)
3. Read frontmatter from each report
4. Show status grouped by state (VERIFIED, DRAFT)
5. Detect draft+audit pairs ready for finalization
6. Generate actionable suggestions

## Status Indicators

| Indicator | Meaning |
|-----------|---------|
| 🟢 | Verified - report is ready for use |
| 🟡 | Draft - needs audit or manual verification |

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| Draft with matching `-audit.md` | "X has audit ready → /research:finalize" |
| Draft without evaluation | "X has no self-critique yet → /research:evaluate" |
| Draft older than 14 days | "X is draft > 14 days → /research:evaluate or audit" |
| No reports | "Start research → /research:investigate <topic>" |

## Configuration File

**Location:** `<targetPath>/.research/config.json`

```json
{
  "targetPath": "./"
}
```
