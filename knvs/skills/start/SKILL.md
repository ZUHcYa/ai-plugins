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
research/        ->  impacts/         ->  ideate/          ->  explore/         ->  exploit/
Pre-Ideate          Impact Analysis     Research            Validation          Scaling

/knvs:impact        /knvs:ideate        /knvs:explore       /knvs:exploit   /knvs:review
extracts            creates             moves &             moves &         quarterly
atomic impacts      new file            adds hypotheses     adds reviews    disruption check
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
        ├── research/
        ├── impacts/
        ├── ideate/
        ├── explore/
        ├── exploit/
        ├── reviews/
        └── .knvs/config.json (targetPath: O/Innovation)

        Ready! Run /knvs:impact or /knvs:ideate to get started.
```

### Without Project Defaults

If no `## KNVS Defaults` in CLAUDE.md, ask interactively:

```
User: /knvs:start
Claude: Welcome to knvs!

        Install here (./)? [Y/n]

User: Y
Claude: Setting up your innovation workspace...

        Created:
        ├── research/
        ├── impacts/
        ├── ideate/
        ├── explore/
        ├── exploit/
        ├── reviews/
        └── .knvs/config.json

        Ready! Run /knvs:impact or /knvs:ideate to get started.
```

### Custom Location

If user answers "n":

```
Claude: Enter target folder path:

User: ./innovation
Claude: Setting up your innovation workspace in ./innovation...

        Created:
        ./innovation/
        ├── research/
        ├── impacts/
        ├── ideate/
        ├── explore/
        ├── exploit/
        ├── reviews/
        └── .knvs/config.json

        Ready! Run /knvs:impact or /knvs:ideate to get started.
```

---

## After Setup (Config Exists)

When config exists, shows combined overview and portfolio:

```
User: /knvs:start
Claude: knvs Status
        ======================================

        RESEARCH (3)
        ------------
        🟢 AI Bookkeeping Analysis [VERIFIED]
        🟡 Fitness Market Study [DRAFT]
        🔵 Vendor X KI-Wandel [HYPOTHESIS]

        IMPACTS (3)
        -----------
        Machine Customers -> 3 atoms (2 High, 1 Medium)
        AI Commerce Trends -> 0 atoms (unprocessed)

        IDEATE (3)
        ----------
        🔴 AI Bookkeeping [WIP] - 45 days stale
        🟡 Fitness App [WIP] - 12 days
        🟢 Invoice Tool [READY] - run /knvs:explore

        EXPLORE (2)
        -----------
        🔴 B2B SaaS - HIGH risk, 2/5 hypotheses validated
        🟢 Mobile App - all hypotheses validated

        EXPLOIT (1)
        -----------
        🟢 Core Business - next review in 3 weeks

        Suggested Actions
        -----------------
        1. AI Commerce Trends verified → /knvs:impact to extract impacts
        2. 3 impacts unused → /knvs:ideate with impact context
        3. Invoice Tool is READY → /knvs:explore
        3. AI Bookkeeping stale → decide: explore or archive
        4. B2B SaaS needs focus → accelerate testing
```

### Empty Project

```
User: /knvs:start
Claude: knvs Status
        ======================================

        IDEATE (0)
        EXPLORE (0)
        EXPLOIT (0)

        Get Started
        -----------
        Add research to research/ and run /knvs:impact to extract impacts,
        or /knvs:ideate to capture your first business idea!
```

---

## What the Skill Does

### First Run (Setup)

1. Check if `.knvs/config.json` exists
2. If NOT:
   a. Search CLAUDE.md for `## KNVS Defaults` section
      - If found: extract `targetPath` (no interactive setup needed)
      - If not found: ask for target path interactively (default: `./`)
   b. Write `.knvs/config.json` with `targetPath`
3. Create folder structure at `targetPath`:
   - `research/` - External research reports (pre-ideate, verified or draft)
   - `impacts/` - Atomic impact analysis from verified research
   - `ideate/` - New ideas and research
   - `explore/` - Ideas being validated
   - `exploit/` - Validated business models
   - `reviews/` - Disruption review history

### After Setup (Overview + Portfolio)

1. Read `.knvs/config.json`
2. Scan folders (`research/`, `impacts/`, `ideate/`, `explore/`, `exploit/`)
3. Read frontmatter from each canvas
4. Calculate priority per phase (see Priority Logic below)
5. Display status grouped by phase
6. Generate actionable suggestions

---

## Priority Logic

| Phase | Priority Calculation | Indicators |
|-------|---------------------|------------|
| RESEARCH | `status (VERIFIED > DRAFT > HYPOTHESIS)` | 🟢 VERIFIED, 🟡 DRAFT, 🔵 HYPOTHESIS |
| IMPACTS | `severity_count * unlinked_ratio` | Count per research, severity breakdown |
| IDEATE | `age_days * (progress == WIP ? 1.5 : 1.0)` | 🔴 >30 days stale, 🟡 active, 🟢 READY |
| EXPLORE | `innovation_risk * potential_revenue` | 🔴 HIGH risk, 🟡 MEDIUM, 🟢 LOW/validated |
| EXPLOIT | `disruption_risk * next_review proximity` | 🔴 review overdue, 🟡 soon, 🟢 on track |

### Status Indicators

| Indicator | Meaning |
|-----------|---------|
| 🔴 | Immediate action needed |
| 🟡 | Monitor closely |
| 🟢 | On track |
| 🔵 | Research assignment open (Hypothesis) |

---

## Suggested Actions Logic

| Condition | Suggestion |
|-----------|------------|
| RESEARCH `status: verified` without impacts | "X verified → /knvs:impact to extract impacts" |
| RESEARCH `status: verified` with impacts | "X verified + impacts → /knvs:ideate to create canvas" |
| RESEARCH `status: draft` | "X draft → /knvs:impact to extract impacts" |
| RESEARCH `status: hypothesis` older than 14 days | "Hypothesis X stale → research or discard" |
| RESEARCH `status: hypothesis` + matching `-verified` report exists | "Hypothesis X possibly resolved by Research Y → verify and delete" |
| IMPACTS exist but no IDEATE canvas links them | "X impacts unused → /knvs:ideate with impact context" |
| IDEATE `progress: READY FOR EXPLORE` | "X is READY → /knvs:explore" |
| IDEATE item > 30 days old | "X stale → decide: explore or archive" |
| EXPLORE with HIGH risk | "X needs focus → accelerate testing" |
| EXPLORE all hypotheses validated | "X ready → /knvs:exploit" |
| EXPLOIT `next_review` within 7 days | "X review due → /knvs:review" |
| RESEARCH `status: draft` with matching `-audit.md` | "X has audit ready → /knvs:finalize" |
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
├── research/
├── impacts/
├── ideate/
├── explore/
├── exploit/
├── reviews/
└── .knvs/
    └── config.json
```

Template files are generated on-demand from the canonical templates embedded in each skill's SKILL.md.

---

## Notes

- Single command to remember: just `/knvs:start`
- Context-aware: detects first run vs. returning user
- Actionable suggestions help users know what to do next
- Shows only top suggestions to avoid overwhelm
