# knvs: Plugin-Specific Rules

## Design-Boundary: Strategyzer-Konzept als Leitplanke

**Harte Regel fuer alle Aenderungen am KNVS-Plugin:**

Jedes neue Feature, jeder neue Skill, jede Erweiterung muss gegen den Strategyzer-Loop geprueft werden, bevor sie umgesetzt wird:

```
Ideate (BMC) → Hypothesize (D/F/V) → Prioritize (Evidence × Importance)
→ Experiment → Learn (Insights) → Decide (Persevere / Pivot / Kill)
```

**Prueffragen bei neuen Features:**
1. Welchem Schritt im Loop gehoert dieses Feature?
2. Ist es in "Testing Business Ideas" oder "The Invincible Company" verankert?
3. Macht es den Loop klarer oder komplexer?
4. Koennte es den Loop verzerren oder einen Schritt ueberproportional aufblaehen?

**Wenn ein Feature keinem Loop-Schritt zugeordnet werden kann → nicht umsetzen.**
**Wenn ein Feature den Loop verkompliziert statt vereinfacht → hinterfragen.**

---

## Vision

**knvs** is a template for Innovation Managers based on the Strategyzer "Testing Business Ideas" and "The Invincible Company" process.

### The Strategyzer Loop (EXPLORE only)

```
IDEATE → HYPOTHESIZE → PRIORITIZE → EXPERIMENT → LEARN → DECIDE
  BMC      D/F/V      Evidence ×     Run tests    Extract   Persevere
                      Importance                  Insights  Pivot / Kill
```

**This loop applies exclusively to EXPLORE canvases.** EXPLOIT canvases do not use hypotheses, experiments, insights, or Persevere/Pivot/Kill decisions — they have their own mechanisms (e.g. Disruption Reviews via `/knvs:review`).

**Phases (The Invincible Company):**
- **EXPLORE** — Create Business Model Canvas (draft), then validate hypotheses through experiments. The full Strategyzer Loop applies here.
- **EXPLOIT** — Validated, running business models. No hypothesis loop — own mechanisms (Performance Assessments via `/knvs:assess`, Disruption Reviews via `/knvs:review`).

New canvases start as `status: draft` in `explore/`. When the BMC is complete, the user sets `status: testing` to begin the validation loop (Hypothesize → Experiment → Learn → Decide). This validation loop is exclusive to the Explore phase.

### Strict Rules

- **Development support only** for knvs (code gen, docs, tests, automation)
- **No user features** that Innovation Managers would use directly
- **No automatic deployments** without review

### Versioning

`CHANGELOG.md` is the single source of truth for the version. `plugin.json` is synced automatically via git pre-commit hook — never edit it manually.

| Change type | Version bump |
|-------------|-------------|
| New feature, Breaking change in Skills | Minor (`x.y.0`) |
| Bugfix, docs correction | Patch (`x.y.z`) |

Always add a `## [x.y.z] - YYYY-MM-DD` entry to `CHANGELOG.md` before committing.

---

## Markdown Generation

**IMPORTANT:** Innovation Managers use Markdown viewers (Obsidian, Notion, VS Code) AND Claude Code reads the contents.

**Target audience:**
- **Primary:** Innovation Managers (humans with a Markdown viewer)
- **Secondary:** Claude Code / AI (must be able to parse the contents)

All generated files must work well in both contexts.

---

## File Naming Conventions

| Type | Format | Example |
|-----|--------|---------|
| Canvas (all phases) | `[slug].md` | `ai-bookkeeping.md` |
| Hypothesis | `[slug].md` | `customers-will-pay-monthly.md` |
| Experiment | `[slug].md` | `pricing-survey-freelancers.md` |
| Insight | `[slug].md` | `freelancers-prefer-annual.md` |

**Rule:** NO date prefix. NO numbering. kebab-case slug only.
CORRECT: `ai-bookkeeping-app.md` — WRONG: `20260220-001-ai-bookkeeping-app.md`

---

## Business Model Canvas: Required Fields

Every Canvas MUST contain these 9 core fields (Osterwalder/Pigneur), as `##` headings:

- **Value Proposition** - What value do we deliver? What problem do we solve?
- **Customer Segments** - For whom are we creating value?
- **Channels** - How do we reach our customers?
- **Customer Relationships** - What relationship does each segment expect?
- **Revenue Streams** - For what are customers willing to pay?
- **Key Resources** - What resources does our value proposition require?
- **Key Activities** - What activities does our value proposition require?
- **Key Partnerships** - Who are our most important partners and suppliers?
- **Cost Structure** - What are the most important costs?

**Missing fields = invalid Canvas.** `/knvs:start` checks for this (Health Checks).
**No callouts on Canvas files.** Callouts (`[!note]`, `[!warning]`, etc.) are not part of the BMC spec. Canvas content = YAML frontmatter + 9 BMC headings + optional sections (Decisions, Notes, Next Steps).

### Status Values

| Status | Folder | Description |
|--------|--------|-------------|
| `draft` | `explore/` | BMC being filled out, not yet in validation |
| `testing` | `explore/` | Actively validating hypotheses through experiments |
| `validated` | `explore/` | All critical hypotheses validated, ready for EXPLOIT |
| `scaling` | `exploit/` | Validated business model being scaled |

### Phase-Specific Extensions

- **Draft (explore/):** Frontmatter `status: draft`, `created`, `updated`. Optional: `pivot_from`. Sections: Decisions, Notes, Next Steps
- **Testing (explore/):** Frontmatter additionally `innovation_risk`, `potential_revenue`. Sections: Decisions, Next Steps
- **Scaling (exploit/):** Frontmatter additionally `next_review`, `disruption_risk`, `revenue_score`, `performance_score`, `trend_score`, `last_assessment`. Sections: Reviews, Decisions, Next Steps

Canonical templates for each phase are in the respective `skills/*/SKILL.md` under `## Canvas Template`.

---

## Hypothesis: Structure

**Hypotheses apply exclusively to EXPLORE canvases.** EXPLOIT canvases do not use hypotheses — they have their own mechanisms.

Hypotheses are testable assumptions extracted from a Business Model Canvas.
Each hypothesis belongs to one of three categories (Strategyzer):

| Category | BMC Fields | Core Question |
|----------|-----------|---------------|
| **Desirability** | Value Proposition, Customer Segments, Channels, Customer Relationships | Do customers want this? |
| **Feasibility** | Key Resources, Key Activities, Key Partnerships | Can we build/deliver this? |
| **Viability** | Revenue Streams, Cost Structure | Is this financially sustainable? |

### Prioritization (2x2 Matrix via Frontmatter)

Hypotheses are prioritized by two dimensions:

| Field | Values | Meaning |
|-------|--------|---------|
| `evidence` | none / weak / strong | How much evidence do we have? |
| `importance` | low / medium / high | How critical is this for the business model? |

**Priority rule:** Test hypotheses with `importance: high` + `evidence: none` first.

### Required Fields (Frontmatter)

- `type: hypothesis`
- `title` (testable statement)
- `canvas` (path to the canvas, e.g. `explore/my-canvas.md`)
- `category` (desirability | feasibility | viability)
- `bmc_fields` (array of affected BMC fields, canonical English names)
- `evidence` (none | weak | strong)
- `importance` (low | medium | high)
- `status` (open | testing | validated | invalidated)
- `created` (YYYY-MM-DD)
- `updated` (YYYY-MM-DD)
- `source_research` (optional, path to verified research report — only for hypotheses derived from external research)

### Required Sections

- Claim (testable statement with context)
- Context (why this matters, which BMC dimensions are affected)
- Evidence (summary of evidence gathered, updated after experiments)
- Related Experiments (links to experiment files)

### Hypothesis Template

```markdown
---
type: hypothesis
title: "Customers will pay EUR 15-30/month for automated bookkeeping"
canvas: explore/ai-bookkeeping.md
category: viability
bmc_fields:
  - Revenue Streams
  - Customer Segments
evidence: none
importance: high
status: open
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Hypothesis: Customers will pay EUR 15-30/month

## Claim

Freelancers in Germany are willing to pay EUR 15-30/month for an AI-powered
bookkeeping solution that automates receipt categorization and tax preparation.

## Context

Revenue Streams in the BMC assume a subscription model at this price point.
If freelancers are not willing to pay this amount, the entire revenue model
needs rethinking.

## Evidence

No evidence yet. Requires pricing experiment.

## Related Experiments

(none yet)
```

Canonical template is in `skills/hypothesize/SKILL.md`.

---

## Experiment: Structure

Experiments test hypotheses through structured methods. The plugin offers predefined
experiment types from "Testing Business Ideas" and allows custom types.

### Predefined Experiment Types

| Category | Types |
|----------|-------|
| **Discovery** | Customer Interview, Online Survey, Landing Page / Smoke Test, Data Analysis, Paper Prototype |
| **Validation** | Concierge MVP, Wizard of Oz, Single Feature MVP, Pre-Sale, Crowdfunding |
| **Custom** | Any user-defined experiment type |

### Required Fields (Frontmatter)

- `type: experiment`
- `title`
- `canvas` (path to the canvas)
- `hypothesis` (path to the hypothesis file)
- `experiment_type` (predefined or custom string)
- `status` (designed | running | completed)
- `created` (YYYY-MM-DD)
- `completed` (YYYY-MM-DD, when done)
- `success_criteria` (measurable outcome)
- `duration` (timeframe)
- `result` (success | failure | inconclusive, after completion)

### Required Sections

- Setup (what we are testing and how)
- Success Criteria (measurable outcomes)
- Process (steps to run the experiment)
- Results (raw data and observations, filled after completion)
- Conclusion (does this validate or invalidate the hypothesis?)

### Experiment Template

```markdown
---
type: experiment
title: "Pricing Survey with 50 Freelancers"
canvas: explore/ai-bookkeeping.md
hypothesis: hypotheses/ai-bookkeeping/customers-will-pay-monthly.md
experiment_type: online-survey
status: designed
created: YYYY-MM-DD
success_criteria: ">60% of respondents willing to pay EUR 15+"
duration: "2 weeks"
---

# Experiment: Pricing Survey with 50 Freelancers

## Setup

Online survey targeting freelancers in Germany to validate willingness
to pay for AI-powered bookkeeping.

## Success Criteria

- >60% of respondents indicate willingness to pay EUR 15+/month
- Sample size: minimum 50 respondents
- Target group: German freelancers with annual revenue < EUR 100k

## Process

1. Create survey with pricing questions
2. Distribute via freelancer communities and LinkedIn
3. Collect responses over 2 weeks
4. Analyze results

## Results

(to be filled after experiment completion)

## Conclusion

(to be filled after experiment completion)
```

Canonical template is in `skills/experiment/SKILL.md`.

---

## Insight: Structure

Insights are key learnings distilled from completed experiments.
Each insight is a standalone file, reusable across canvas boundaries.

### Required Fields (Frontmatter)

- `type: insight`
- `title` (key learning, concise)
- `source_experiment` (path to the experiment file)
- `canvas` (path to the canvas)
- `bmc_fields` (array of affected BMC fields)
- `created` (YYYY-MM-DD)
- `tags` (array, categorization)

### Required Sections

- Learning (what we learned)
- Evidence (data/observations that support this)
- Implications (how this affects the business model)
- Source (link to experiment)

### Insight Template

```markdown
---
type: insight
title: "Freelancers prefer annual billing with discount"
source_experiment: experiments/ai-bookkeeping/pricing-survey-freelancers.md
canvas: explore/ai-bookkeeping.md
bmc_fields:
  - Revenue Streams
created: YYYY-MM-DD
tags: [pricing, customer-preference]
---

# Insight: Freelancers prefer annual billing with discount

## Learning

72% of surveyed freelancers prefer annual billing (EUR 149/year)
over monthly billing (EUR 15/month). The 17% discount is perceived
as significant value.

## Evidence

- Survey: 50 respondents, 72% chose annual option
- Common feedback: "Monthly subscriptions add up"
- Price sensitivity highest in EUR 20-30/month range

## Implications

Revenue Streams should offer both monthly and annual pricing,
with annual as the default/recommended option. This may affect
cash flow projections in the BMC.

## Source

[[experiments/ai-bookkeeping/pricing-survey-freelancers.md]]
```

Canonical template is in `skills/learn/SKILL.md`.

---

## Assessment: Structure

**Assessments apply exclusively to EXPLOIT canvases.** EXPLORE canvases use the hypothesis loop instead.

Two assessment types provide complementary perspectives ("The Invincible Company"):

- **Performance** — Current snapshot (Ist-Aufnahme). 10 BMC-mapped dimensions.
- **Trend** — Future projection (Zukunftsprojektion). 10 external trend dimensions.

Each assessment scores 10 dimensions from -3 to +3, total score: -30 to +30.

### Performance Assessment: 10 Dimensions

Each dimension maps to a BMC field:

| # | Dimension | -3 Statement | +3 Statement |
|---|-----------|-------------|-------------|
| 1 | **Value Proposition** | Products/services perform worse than competition | Highly differentiated and loved by customers |
| 2 | **Customer Segments** | Lost 20%+ of customer base in last 6 months | Increased customer base by 50%+ in last 6 months |
| 3 | **Channels** | 100% dependent on intermediaries, difficult market access | Direct market access, fully own customer relationship |
| 4 | **Customer Relationships** | Customers could leave immediately, no switching costs | Customers locked in for years, significant switching costs |
| 5 | **Key Resources** | Inferior to competitors, deteriorated. New entrants with better/cheaper resources | Can't be copied/emulated for years, competitive advantage (IP, brand) |
| 6 | **Key Activities** | Inferior to competitors, deteriorated. New entrants with better/cheaper activities | Can't be copied/emulated for years, competitive advantage (cost effectiveness, scale) |
| 7 | **Key Partnerships** | Lost access to key partners in last 6 months | Key partners locked in for years |
| 8 | **Revenue Streams** | Lost 20%+ of revenues in last 6 months | Doubled revenues in last 6 months, growing faster than competitors |
| 9 | **Cost Structure** | Costs grew faster than revenues, less effective than competitors | Costs shrunk vs revenue, more effective than competitors |
| 10 | **Margins** | Margins shrunk 50%+ and/or 50%+ lower than competition | Margins increased 50%+ and/or 50%+ higher than competition |

### Trend Assessment: 10 Dimensions

Each dimension assesses an external threat or opportunity:

| # | Dimension | -3 Statement | +3 Statement |
|---|-----------|-------------|-------------|
| 1 | **Substitution Risk** | New entrants gaining traction with cheaper/better/substitute products | Competition shrinking, products likely to gain traction |
| 2 | **Market Trajectory** | Markets projected to shrink significantly | Markets projected to grow significantly |
| 3 | **Customer Friction** | Trends reducing friction for customers to leave | Trends increasing friction for customers to leave |
| 4 | **Social/Cultural Trends** | Growing trends driving customers away (sustainability, fashion, etc.) | Trends making it harder for customers to desert us |
| 5 | **Technology Trends** | Tech trends undermining/making model obsolete | Tech trends strengthening model |
| 6 | **Regulatory Environment** | New regulations making model more expensive/impossible | New regulations making model cheaper/easier |
| 7 | **Supply Chain** | Suppliers/value chain changing to put model at risk | Suppliers/value chain changing to strengthen model |
| 8 | **Economic Resilience** | Economic downturn would be lethal | Model is resilient, would benefit from downturn |
| 9 | **Geopolitical Dependencies** | Model depends on geopolitically affected resources | Model doesn't depend on geopolitically affected resources |
| 10 | **VC/Startup Activity** | Significant VC funding startups in our arena, growing | Little to no VC funding in our arena |

### Required Fields (Frontmatter)

- `type: assessment`
- `assessment_type` (`performance` or `trend`)
- `canvas` (path to the exploit canvas)
- `performance_score` or `trend_score` (integer, -30 to +30)
- `date` (YYYY-MM-DD)

### File Naming Convention

Assessment filenames include the type suffix:
- `YYYY-MM-DD-performance.md`
- `YYYY-MM-DD-trend.md`

### Required Sections

- Scores (table with Dimension, Score, Note)
- Trends (comparison with previous assessment, if available)
- Summary (narrative of health or risk assessment)

### Performance Assessment Template

```markdown
---
type: assessment
assessment_type: performance
canvas: exploit/canvas-slug.md
performance_score: 8
date: YYYY-MM-DD
---

# Performance Assessment: [Canvas Title]

## Date

YYYY-MM-DD

## Scores

| # | Dimension | Score | Note |
|---|-----------|-------|------|
| 1 | Value Proposition | +2 | |
| 2 | Customer Segments | +1 | |
| 3 | Channels | -1 | |
| 4 | Customer Relationships | 0 | |
| 5 | Key Resources | +2 | |
| 6 | Key Activities | +1 | |
| 7 | Key Partnerships | +1 | |
| 8 | Revenue Streams | +2 | |
| 9 | Cost Structure | -1 | |
| 10 | Margins | +1 | |

**Performance Score: +8**

## Trends

(compared to previous assessment, if available)

## Summary

[Brief narrative of overall business model health and key observations]
```

### Trend Assessment Template

```markdown
---
type: assessment
assessment_type: trend
canvas: exploit/canvas-slug.md
trend_score: 2
date: YYYY-MM-DD
---

# Trend Assessment: [Canvas Title]

## Date

YYYY-MM-DD

## Scores

| # | Dimension | Score | Note |
|---|-----------|-------|------|
| 1 | Substitution Risk | -2 | |
| 2 | Market Trajectory | +2 | |
| 3 | Customer Friction | +1 | |
| 4 | Social/Cultural Trends | 0 | |
| 5 | Technology Trends | -1 | |
| 6 | Regulatory Environment | +1 | |
| 7 | Supply Chain | 0 | |
| 8 | Economic Resilience | +1 | |
| 9 | Geopolitical Dependencies | +2 | |
| 10 | VC/Startup Activity | -2 | |

**Trend Score: +2**

## Trends

(compared to previous assessment, if available)

## Summary

[Brief narrative of external threats, opportunities, and disruption risk]
```

Canonical templates are in `skills/assess/SKILL.md`.

---

## Decision Log: Format

Decisions are documented in a `## Decisions` section directly in the canvas file.
Each decision records the Persevere/Pivot/Kill outcome with reasoning.

### Decision Entry Format

```markdown
### YYYY-MM-DD — [Persevere|Pivot|Kill]

**Kontext:** [Summary of evidence and hypothesis status]
**Entscheidung:** [Reasoning for the decision]
**Naechste Schritte:** [What happens next]
```

### Pivot Mechanics

When a Pivot decision is made:
1. A new canvas is created in `explore/` with `status: draft` and `pivot_from: explore/original.md` in frontmatter
2. The original canvas is moved to `archive/`
3. The decision is logged in the new canvas

### Kill Mechanics

When a Kill decision is made:
1. The canvas is moved to `archive/`
2. Hypotheses, experiments, and insights remain for reference

---

## Folder Structure

```
.knvs/config.json
explore/             — BMCs (draft and testing)
exploit/             — Validated, running business models (scaling)
hypotheses/          — Hypotheses, grouped by canvas
  <canvas-slug>/
experiments/         — Experiments, grouped by canvas
  <canvas-slug>/
insights/            — Insights, grouped by canvas
  <canvas-slug>/
reviews/             — Disruption reviews
  <canvas-slug>/
assessments/         — Performance & Trend assessments
  <canvas-slug>/
archive/             — Killed/pivoted canvases
```

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/knvs:` prefix | `/knvs:ideate`, `/knvs:hypothesize`, `/knvs:start` |
| Skill files | `<skill>/SKILL.md` | `skills/ideate/SKILL.md`, `skills/hypothesize/SKILL.md` |
| Configuration | `.knvs/` folder | `.knvs/config.json` |
| Phase folders | Short, unambiguous | `explore/`, `exploit/` |
| Data folders | Descriptive | `hypotheses/`, `experiments/`, `insights/`, `archive/` |

---

## Emoji Usage

**Rule:** Emojis only when they provide real value - not as decoration.

### Allowed
- Status indicators where text alone would be unclear (e.g. in compact tables)
- Phase symbols for quick visual orientation

### Forbidden
- Decorative emojis without function
- Multiple emojis per line/heading
- Emojis in body text

---

## Tool-Agnostic: Manual Workflow Always Possible

**Core principle:** The user must always be able to carry out the knvs process **manually without skills**.

**Rules:**
- All files are pure Markdown with standard frontmatter
- Skills are helpers, not a prerequisite
- Users can create, move, and edit files manually
- Folder structure and file format are self-explanatory

**Forbidden:**
- Obsidian-specific plugins or Dataview queries as core functionality
- Proprietary file formats
- Features that only work with Claude Code
- Dependencies on external APIs or services
