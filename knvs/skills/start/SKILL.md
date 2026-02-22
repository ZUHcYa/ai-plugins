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
ideate/          ->  explore/                                    ->  exploit/
Ideation            Validation Loop                                Scaling

/knvs:ideate        /knvs:explore    /knvs:hypothesize           /knvs:exploit   /knvs:review
creates             moves &          extracts D/F/V              moves &         quarterly
new BMC             begins loop      hypotheses                  adds reviews    disruption check

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
        ├── ideate/
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
        ├── ideate/
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

        IDEATE (2)
        ----------
        🔴 AI Bookkeeping [WIP] - 45 days stale
        🟢 Invoice Tool [READY] - run /knvs:explore

        EXPLORE (2)
        -----------
        🔴 B2B SaaS - 2/5 hypotheses validated, 1 experiment stale
        🟢 Mobile App - all hypotheses validated → /knvs:exploit

        EXPLOIT (1)
        -----------
        🟢 Core Business - next review in 3 weeks

        Suggested Actions
        -----------------
        1. Invoice Tool is READY → /knvs:explore
        2. B2B SaaS: stale experiment → check progress
        3. Mobile App: all validated → /knvs:exploit
        4. AI Bookkeeping stale → decide: explore or archive
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
   - `ideate/` - New ideas being researched
   - `explore/` - Ideas being validated with experiments
   - `exploit/` - Validated business models being scaled
   - `hypotheses/` - Hypotheses grouped by canvas
   - `experiments/` - Experiments grouped by canvas
   - `insights/` - Insights grouped by canvas
   - `reviews/` - Disruption review history
   - `archive/` - Killed/pivoted canvases

### After Setup (Overview + Portfolio)

1. Read `.knvs/config.json`
2. Scan folders (`ideate/`, `explore/`, `exploit/`)
3. For EXPLORE canvases: scan `hypotheses/<slug>/` and `experiments/<slug>/` for status
4. Read frontmatter from each canvas
5. Calculate priority per phase (see Priority Logic below)
6. Display status grouped by phase
7. Generate actionable suggestions

---

## Priority Logic

| Phase | Priority Calculation | Indicators |
|-------|---------------------|------------|
| IDEATE | `age_days * (progress == WIP ? 1.5 : 1.0)` | 🔴 >30 days stale, 🟡 active, 🟢 READY |
| EXPLORE | `hypothesis_validation_ratio + stale_experiments` | 🔴 stale experiments, 🟡 testing, 🟢 all validated |
| EXPLOIT | `disruption_risk * next_review proximity` | 🔴 review overdue, 🟡 soon, 🟢 on track |

### Status Indicators

| Indicator | Meaning |
|-----------|---------|
| 🔴 | Immediate action needed |
| 🟡 | Monitor closely |
| 🟢 | On track |

---

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| IDEATE `progress: READY FOR EXPLORE` | "X is READY → /knvs:explore" |
| IDEATE item > 30 days old | "X stale → decide: explore or archive" |
| EXPLORE with no hypotheses | "X has no hypotheses → /knvs:hypothesize" |
| EXPLORE with open hypotheses, no experiment | "X has untested hypotheses → /knvs:experiment" |
| EXPLORE with completed experiment, no insights | "X has experiment results → /knvs:learn" |
| EXPLORE with stale experiment | "X has stale experiment → check progress" |
| EXPLORE all hypotheses validated | "X ready → /knvs:exploit" |
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
├── ideate/
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
