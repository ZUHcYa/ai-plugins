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

/knvs:ideate        /knvs:hypothesize     /knvs:exploit     /knvs:review
creates BMC         extracts D/F/V        moves &           quarterly
as draft            hypotheses            adds reviews      disruption check

                    /knvs:experiment
                    designs & runs tests

                    /knvs:learn
                    extracts insights

                    /knvs:decide
                    persevere / pivot / kill
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
        ├── hypotheses/
        ├── experiments/
        ├── insights/
        ├── reviews/
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
        ├── hypotheses/
        ├── experiments/
        ├── insights/
        ├── reviews/
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
        🔴 AI Bookkeeping [draft] - 45 days stale
        🟢 Invoice Tool [draft] - created 3 days ago

        TESTING (2)
        -----------
        🔴 B2B SaaS - 2/5 hypotheses validated, 1 experiment stale
        🟢 Mobile App - all hypotheses validated → /knvs:exploit

        EXPLOIT (1)
        -----------
        🟢 Core Business - next review in 3 weeks

        Health (2 issues)
        -----------------
        - explore/ai-bookkeeping.md: Missing BMC field "Key Partnerships"
        - hypotheses/old-idea/price-sensitivity.md: Canvas not found
          → Orphaned hypothesis — archive or delete?

        Suggested Actions
        -----------------
        1. Mobile App: all validated → /knvs:exploit
        2. B2B SaaS: stale experiment → check progress
        3. AI Bookkeeping stale → set status: testing or archive
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
   - `hypotheses/` - Hypotheses grouped by canvas
   - `experiments/` - Experiments grouped by canvas
   - `insights/` - Insights grouped by canvas
   - `reviews/` - Disruption review history
   - `archive/` - Killed/pivoted canvases

### After Setup (Overview + Portfolio)

1. Read `.knvs/config.json`
2. Scan `explore/` and `exploit/` folders
3. Group `explore/` canvases by status (`draft` vs `testing`)
4. For testing canvases: scan `hypotheses/<slug>/` and `experiments/<slug>/` for status
5. Read frontmatter from each canvas
6. Calculate priority per group (see Priority Logic below)
7. Run Health Checks (see Health Checks below)
8. Display status grouped by phase
9. Show Health issues (if any — omit section when 0 issues)
10. Generate actionable suggestions

---

## Priority Logic

| Group | Priority Calculation | Indicators |
|-------|---------------------|------------|
| DRAFTS | `age_days` | 🔴 >30 days stale, 🟡 active, 🟢 recent |
| TESTING | `hypothesis_validation_ratio + stale_experiments` | 🔴 stale experiments, 🟡 testing, 🟢 all validated |
| EXPLOIT | `disruption_risk * next_review proximity` | 🔴 review overdue, 🟡 soon, 🟢 on track |

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
| 1 | Invalid Canvas | `explore/`, `exploit/` | `.md` without valid YAML frontmatter | `"Invalid frontmatter"` |
| 2 | Status ↔ Folder | `explore/`, `exploit/` | Canvas folder does not match status | `"Status X does not match folder Y"` |
| 3 | Missing BMC Fields | `explore/`, `exploit/` | Canvas lacks one of the 9 core `##` headings | `"Missing BMC field: X"` |
| 4 | Missing Frontmatter | `explore/` (testing) | Testing canvas without `innovation_risk` | `"Missing field: innovation_risk"` |
| 5 | Hypothesis Missing Sections | `hypotheses/` | Without `## Claim` or `## Context` | `"Missing section: X"` |
| 6 | Orphaned Hypothesis | `hypotheses/` | `canvas` frontmatter points to non-existent file | `"Canvas X not found"` |
| 7 | Experiment → Hypothesis | `experiments/` | `hypothesis` frontmatter points to non-existent file | `"Hypothesis X not found"` |
| 8 | Insight → Experiment | `insights/` | `source_experiment` points to non-existent file | `"Experiment X not found"` |
| 9 | Orphaned Data Folder | `hypotheses/`, `experiments/` | Subfolder `<slug>/` has no matching canvas in `explore/` or `exploit/` | `"No matching canvas for <slug>/"` |
| 10 | Hypothesis → Research | `hypotheses/` | `source_research` points to non-existent or non-verified file | `"Research X not found or not verified"` |

---

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| Draft > 30 days old | "X stale → set status: testing or archive" |
| Testing with no hypotheses | "X has no hypotheses → /knvs:hypothesize" |
| Testing with open hypotheses, no experiment | "X has untested hypotheses → /knvs:experiment" |
| Testing with completed experiment, no insights | "X has experiment results → /knvs:learn" |
| Testing with stale experiment | "X has stale experiment → check progress" |
| Testing all hypotheses validated | "X ready → /knvs:exploit" |
| EXPLOIT `next_review` within 7 days | "X review due → /knvs:review" |
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
├── hypotheses/
├── experiments/
├── insights/
├── reviews/
├── archive/
└── .knvs/
    └── config.json
```

---

## Notes

- Single command to remember: just `/knvs:start`
- Context-aware: detects first run vs. returning user
- Actionable suggestions help users know what to do next
- Shows only top suggestions to avoid overwhelm
