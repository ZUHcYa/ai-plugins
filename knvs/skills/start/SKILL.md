---
name: "start"
description: "Smart knvs entry point - setup on first run, overview afterwards"
---

# /knvs:start

## Purpose

Single entry point for knvs. Automatically detects context:
- **First run:** Sets up folder structure and config
- **After setup:** Shows combined overview and portfolio

## Phase Lifecycle

```
explore/                                                    exploit/
(status: draft -> testing)                                  (status: scaling)

/knvs:ideate        /knvs:hypothesize     /knvs:exploit     /knvs:assess
creates BMC         extracts D/F/V        moves &           performance &
as draft            hypotheses            scales            trend assessment

                    /knvs:experiment
                    designs & runs tests

                    /knvs:learn
                    extracts insights

                    /knvs:card
                    learning card + decision
                    (persevere / pivot / kill)
```

## When to Use

- Starting with knvs for the first time
- Beginning a session - get oriented quickly
- After a break - see where you left off
- Quick status check - what needs attention?

---

## First Run (No Config)

When no `.knvs/config.json` exists, setup runs automatically.

### With Project Defaults

Check the project's CLAUDE.md for a `## KNVS Defaults` section. If found, use the
pre-configured `targetPath` value - no interactive setup needed:

```
User: /knvs:start
Claude: Welcome to knvs!
        [targetPath taken from project defaults]

        Created:
        ├── explore/
        ├── exploit/
        ├── archive/
        └── .knvs/config.json

        Ready! Run /knvs:ideate to capture your first business idea.
```

### Without Project Defaults

```
User: /knvs:start
Claude: Welcome to knvs!

        Install here (./)? [Y/n]

User: Y
Claude: Setting up your innovation workspace...

        Created:
        ├── explore/
        ├── exploit/
        ├── archive/
        └── .knvs/config.json

        Ready! Run /knvs:ideate to capture your first business idea.
```

---

## After Setup (Config Exists)

When config exists, shows combined overview and portfolio:

```
User: /knvs:start
Claude: knvs Status
        ======================================

        DRAFTS (2)
        ----------
        🔴 AI Bookkeeping [draft] risk:high revenue:— — 45 days stale
        🟢 Invoice Tool [draft] risk:high revenue:moderate — created 3 days ago

        TESTING (2)
        -----------
        🔴 B2B SaaS [testing] risk:high revenue:high — 2/5 hypotheses validated, 1 experiment stale
        🟢 Mobile App [testing] risk:low revenue:moderate — all validated → /knvs:exploit

        EXPLOIT (2)
        -----------
        🔴 AI Bookkeeping [scaling] risk:high revenue:high — P:-5 T:-3
        🟢 Core Business [scaling] risk:low revenue:moderate — P:+12 T:+8

        Health (2 issues)
        -----------------
        - explore/ai-bookkeeping/ai-bookkeeping.md: Missing BMC field "Key Partnerships"
        - explore/old-idea/hypotheses/price-sensitivity.md: Canvas file not found
          → Canvas folder exists without canvas file

        Suggested Actions
        -----------------
        1. Mobile App: all validated → /knvs:exploit
        2. AI Bookkeeping (exploit): risk:high (P:-5 T:-3) → urgent review needed
        3. B2B SaaS: stale experiment → check progress
        4. AI Bookkeeping (explore): stale → set status: testing or archive
```

---

## What the Skill Does

### First Run (Setup)

1. Check if `.knvs/config.json` exists
2. If NOT:
   a. Search CLAUDE.md for `## KNVS Defaults` section
      - If found: extract `targetPath`
      - If not found: ask interactively (default: `./`)
   b. Write `.knvs/config.json` with `targetPath`
3. Create folder structure at `targetPath`:
   - `explore/` - Business Model Canvases (draft and testing)
   - `exploit/` - Validated business models being scaled
   - `archive/` - Killed/pivoted canvases
   Canvas subfolders (hypotheses/, experiments/, etc.) are created on-demand by the respective skills.

### After Setup (Overview + Portfolio)

1. Read `.knvs/config.json`
2. Scan `explore/` and `exploit/` folders
3. Group `explore/` canvases by status (`draft` vs `testing`)
4. For testing canvases: scan `explore/<slug>/hypotheses/` and `explore/<slug>/experiments/` for status
5. Read frontmatter from each canvas (`risk`, `revenue`, `performance_score`, `trend_score`)
6. Calculate priority per group (see Priority Logic below) — TESTING and EXPLOIT use `risk` field
7. Run Health Checks (see Health Checks below)
8. Display status grouped by phase
9. Show Health issues (if any — omit section when 0 issues)
10. Generate actionable suggestions

---

## Priority Logic

| Group | Priority Calculation | Indicators |
|-------|---------------------|------------|
| DRAFTS | `age_days` | 🔴 >30 days stale, 🟡 active, 🟢 recent |
| TESTING | `risk field + stale_experiments` | 🔴 risk:high, 🟡 risk:moderate, 🟢 risk:low |
| EXPLOIT | `risk field` | 🔴 risk:high, 🟡 risk:moderate, 🟢 risk:low |

Assessment recency remains a secondary check: if `last_assessment` > 90 days, add to Suggested Actions regardless of risk level.

### Status Indicators

| Indicator | Meaning |
|-----------|---------|
| 🔴 | Immediate action needed |
| 🟡 | Monitor closely |
| 🟢 | On track |

---

## Health Checks

After the portfolio overview, start runs structural integrity checks.
Issues are shown in a **Health** section before Suggested Actions.
If no issues exist, the Health section is omitted entirely.

**Important:** Check 1 (Invalid Canvas) runs first. If a file has invalid frontmatter, skip remaining checks for that file.

| # | Check | Scans | Condition | Message |
|---|-------|-------|-----------|---------|
| 1 | Invalid Canvas | `explore/*/`, `exploit/*/` | Canvas `.md` without valid YAML frontmatter | `"Invalid frontmatter"` |
| 2 | Status ↔ Folder | `explore/*/`, `exploit/*/` | Canvas folder does not match status | `"Status X does not match folder Y"` |
| 3 | Missing BMC Fields | `explore/*/`, `exploit/*/` | Canvas lacks one of the 9 core `##` headings | `"Missing BMC field: X"` |
| 4 | Missing Frontmatter | `explore/*/`, `exploit/*/` | Canvas without `risk` field | `"Missing field: risk"` |
| 5 | Hypothesis Missing Sections | `explore/*/hypotheses/`, `exploit/*/hypotheses/` | Without `## Claim` or `## Context` | `"Missing section: X"` |
| 6 | Missing Canvas File | `explore/*/`, `exploit/*/` | Canvas folder exists but `<slug>.md` is missing | `"Canvas file not found in <slug>/"` |
| 7 | Experiment → Hypothesis | `explore/*/experiments/`, `exploit/*/experiments/` | `hypothesis` frontmatter points to non-existent file (resolved as canvas-relative) | `"Hypothesis X not found"` |
| 8 | Insight → Experiment | `explore/*/insights/`, `exploit/*/insights/` | `source_experiment` points to non-existent file (resolved as canvas-relative) | `"Experiment X not found"` |
| 9 | Hypothesis → Research | `explore/*/hypotheses/`, `exploit/*/hypotheses/` | `source_research` points to non-existent or non-verified file (root-relative) | `"Research X not found or not verified"` |
| 10 | Learning Card → Experiment | `explore/*/learning-cards/`, `exploit/*/learning-cards/` | `experiment` or `experiments` frontmatter points to non-existent file (canvas-relative) | `"Experiment X not found"` |

**Note:** All intra-canvas references (`hypothesis:`, `experiment:`, `source_experiment:`, `insights:`) are resolved relative to the canvas folder. Only `source_research:` is root-relative.

---

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| Draft > 30 days old | "X stale → set status: testing or archive" |
| Testing with no hypotheses | "X has no hypotheses → /knvs:hypothesize" |
| Testing with open hypotheses, no experiment | "X has untested hypotheses → /knvs:experiment" |
| Testing with completed experiment, no insights | "X has experiment results → /knvs:learn" |
| Testing with insights but no learning card | "X has insights → /knvs:card" |
| Testing with stale experiment | "X has stale experiment → check progress" |
| Testing all hypotheses validated | "X ready → /knvs:exploit" |
| EXPLOIT risk:high | "X has high risk (P:score T:score) → urgent review needed" |
| EXPLOIT risk:moderate, trend_score < 0 | "X has declining trends → consider exploring adjacent models" |
| EXPLOIT `last_assessment` > 90 days ago | "X assessment due → /knvs:assess" |
| No items exist | "Run /knvs:ideate to capture your first idea" |

---

## Configuration File

**Location:** `<targetPath>/.knvs/config.json`

```json
{
  "targetPath": "./"
}
```

---

## What Gets Created (First Run)

```
<targetPath>/
├── explore/
├── exploit/
├── archive/
└── .knvs/
    └── config.json
```

Canvas subfolders (`hypotheses/`, `experiments/`, `insights/`, `learning-cards/`, `assessments/`) are created on-demand inside each canvas folder by the respective skills.

---

## Notes

- Single command to remember: just `/knvs:start`
- Context-aware: detects first run vs. returning user
- Actionable suggestions help users know what to do next
- Shows only top suggestions to avoid overwhelm
