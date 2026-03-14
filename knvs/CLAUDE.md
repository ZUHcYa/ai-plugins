# knvs: Plugin-Specific Rules

## Design-Boundary: Innovation Loop als Leitplanke

**Harte Regel fuer alle Aenderungen am KNVS-Plugin:**

Jedes neue Feature, jeder neue Skill, jede Erweiterung muss gegen den Innovation Loop geprueft werden:

```
Ideate → Hypothesize → Prioritize → Test → Learn → Decide
```

**Prueffragen bei neuen Features:**
1. Welchem Schritt im Loop gehoert dieses Feature?
2. Macht es den Loop klarer oder komplexer?
3. Koennte es den Loop verzerren oder einen Schritt ueberproportional aufblaehen?

**Wenn ein Feature keinem Loop-Schritt zugeordnet werden kann → nicht umsetzen.**

---

## Vision

**knvs** is a template for hypothesis-driven business model validation.

### The Innovation Loop (EXPLORE only)

```
IDEATE → HYPOTHESIZE → PRIORITIZE → EXPERIMENT → LEARN → CARD (DECIDE)
Canvas    Extract        Confidence ×   Run tests    Extract   Learning Card
          hypotheses     Importance                  Insights  Continue/Pivot/Stop
```

**This loop applies exclusively to EXPLORE canvases.** EXPLOIT canvases do not use hypotheses, experiments, insights, or Continue/Pivot/Stop decisions — they have their own mechanisms (Performance & Trend Assessments via `/knvs:assess`).

**DECIDE is embedded in the Learning Card.** The Hypothesis Learning Card (`/knvs:card [H]`) concludes the testing cycle of one hypothesis and includes the strategic decision (Continue/Pivot/Stop).

**Phases:**
- **EXPLORE** — Create Canvas (draft), then validate hypotheses through experiments. The full Innovation Loop applies here.
- **EXPLOIT** — Validated, running business models. No hypothesis loop — own mechanisms (Performance & Trend Assessments via `/knvs:assess`).

New canvases start as `status: draft` in `explore/`. When the canvas is complete, the user sets `status: testing` to begin the validation loop. This validation loop is exclusive to the Explore phase.

### Strict Rules

- **Development support only** for knvs (code gen, docs, tests, automation)
- **No user features** that end users would use directly
- **No automatic deployments** without review

### Versioning

`CHANGELOG.md` is the single source of truth for the version. `plugin.json` is synced automatically via git pre-commit hook — never edit it manually.

| Change type | Version bump |
|-------------|-------------|
| New feature, Breaking change in Skills | Minor (`x.y.0`) |
| Bugfix, docs correction | Patch (`x.y.z`) |

Always add a `## [x.y.z] - YYYY-MM-DD` entry to `CHANGELOG.md` before committing.

---

## File Naming Conventions

| Type | Format | Example |
|-----|--------|---------|
| Canvas (all phases) | `[slug]/[slug].md` | `ai-bookkeeping/ai-bookkeeping.md` |
| Hypothesis | `[slug].md` | `customers-will-pay-monthly.md` |
| Experiment | `[slug].md` | `pricing-survey-freelancers.md` |
| Insight | `[slug].md` | `freelancers-prefer-annual.md` |
| Learning Card | `[slug].md` | `customers-will-pay-monthly.md` |

**Rule:** NO date prefix. NO numbering. kebab-case slug only.

---

## Canvas: Structure

### Convention over Configuration

The canvas file describes itself. The `##` headings ARE the schema. Skills read
existing headings instead of validating against a hardcoded list.

A canvas MUST contain **at least 1 `##` content section.** BMC (Business Model Canvas)
with 9 fields is the default template, but not the only option.

### Default BMC Template (9 fields)

The default canvas uses the Osterwalder/Pigneur BMC:
Value Proposition, Customer Segments, Channels, Customer Relationships,
Revenue Streams, Key Resources, Key Activities, Key Partnerships, Cost Structure.

For alternative canvas types, adapt `##` headings to your methodology.

### Frontmatter

- `type: canvas` (required; `bmc` accepted for backward compatibility)
- `canvas_type: bmc` (optional; specifies which canvas methodology is used)
- `status` (draft | testing | validated | scaling)
- `risk` (high | moderate | low)
- `revenue` (high | moderate | low, user-set)
- `created`, `updated` (YYYY-MM-DD)
- `pivot_from` (optional, slug only)

### Status Values

| Status | Folder | Description |
|--------|--------|-------------|
| `draft` | `explore/` | Canvas being filled out, not yet in validation |
| `testing` | `explore/` | Actively validating hypotheses through experiments |
| `validated` | `explore/` | All critical hypotheses validated, ready for EXPLOIT |
| `scaling` | `exploit/` | Validated model being scaled |

### Risk & Revenue: Unified Fields

`risk` and `revenue` are unified frontmatter fields present in ALL canvases.
They use the same 3-level scale in both phases, enabling portfolio-level comparison.

**EXPLORE `risk` derivation** (updated by `/knvs:learn`):
- `high`: Status is `draft`, OR any `importance: high` hypothesis is NOT validated
- `moderate`: All `importance: high` validated, some `importance: medium` NOT validated
- `low`: All `importance: high` and `importance: medium` hypotheses validated

**EXPLOIT `risk` derivation** (updated by `/knvs:assess`):
- `high`: `performance_score < 0` AND `trend_score < 0`, OR no assessment exists
- `moderate`: one score positive, one negative
- `low`: `performance_score >= 0` AND `trend_score >= 0`

Canonical canvas templates are in the respective `skills/*/SKILL.md`.

---

## Hypothesis: Structure

**Hypotheses apply exclusively to EXPLORE canvases.**

Hypotheses are testable assumptions extracted from a canvas. Each hypothesis
has a **category** — free-text, suggested based on canvas headings.

Default category suggestions for BMC canvases:

| Category | BMC Fields | Core Question |
|----------|-----------|---------------|
| **Desirability** | Value Proposition, Customer Segments, Channels, Customer Relationships | Do customers want this? |
| **Feasibility** | Key Resources, Key Activities, Key Partnerships | Can we build/deliver this? |
| **Viability** | Revenue Streams, Cost Structure | Is this financially sustainable? |

For non-BMC canvases, categories are derived from the canvas `##` headings.

### Confidence Level (3 levels)

| Value | Criteria |
|-------|----------|
| *(empty)* | No experiments completed yet |
| `low` | Few experiments or weak evidence only |
| `moderate` | Multiple experiments with some strong evidence |
| `high` | Multiple experiments, at least one with very strong evidence |

**Backward compatibility:** `very-low` is mapped to `low` when reading.

### Prioritization

| Field | Values | Meaning |
|-------|--------|---------|
| `confidence` | *(empty)* / low / moderate / high | How confident are we? |
| `importance` | low / medium / high | How critical for the model? |

**Priority rule:** Test hypotheses with `importance: high` + empty `confidence` first.

### Required Fields (Frontmatter)

- `type: hypothesis`
- `title`, `canvas` (slug), `category` (free-text)
- `canvas_fields` (array; `bmc_fields` accepted for backward compatibility)
- `confidence`, `importance`, `status` (open | testing | validated | invalidated)
- `created`, `updated`
- `source_research` (optional)

### Required Sections

Claim, Context, Evidence, Related Experiments.

Canonical template is in `skills/hypothesize/SKILL.md`.

---

## Experiment: Structure

Experiments test hypotheses through structured methods. Predefined experiment
types are available as suggestions, and custom types are supported.

### Required Fields (Frontmatter)

- `type: experiment`
- `title`, `canvas` (slug), `hypothesis` (canvas-relative path)
- `experiment_type` (predefined or custom string)
- `status` (designed | running | completed)
- `created`, `completed`, `duration`
- `result` (success | failure | inconclusive)
- `evidence_strength` (weak | strong)

### Required Sections

Setup, Success Criteria, Process, Results, Conclusion.

Canonical template is in `skills/experiment/SKILL.md`.

---

## Insight: Structure

Insights are key learnings distilled from completed experiments.
Each insight is a standalone file, reusable across canvas boundaries.

### Required Fields (Frontmatter)

- `type: insight`
- `title`, `source_experiment` (canvas-relative), `canvas` (slug)
- `canvas_fields` (array; `bmc_fields` accepted for backward compatibility)
- `created`, `tags`

### Required Sections

Learning, Evidence, Implications, Source.

Canonical template is in `skills/learn/SKILL.md`.

---

## Learning Card: Structure

**Learning Cards apply exclusively to EXPLORE canvases.**

Two scopes:
- **Experiment Card** (`scope: experiment`) — One card per experiment (1:1)
- **Hypothesis Card** (`scope: hypothesis`) — Concludes testing cycle with Continue/Pivot/Stop decision

### Decision Field

- `decision` on hypothesis cards: `continue | pivot | stop`
- **Backward compatibility:** `persevere` → `continue`, `kill` → `stop` when reading

### Required Sections

- We believed that... / We observed... / From that we learned... / Therefore we will...

Canonical templates are in `skills/card/SKILL.md`.

---

## Assessment: Structure

**Assessments apply exclusively to EXPLOIT canvases.**

Two types provide complementary perspectives:
- **Performance** — Current snapshot. 10 dimensions mapped to canvas fields.
- **Trend** — Future projection. 10 external trend dimensions.

Each assessment scores 10 dimensions from -3 to +3, total score: -30 to +30.
The specific dimensions are defined in `skills/assess/SKILL.md`.

### Required Fields (Frontmatter)

- `type: assessment`
- `canvas` (slug)
- `performance_score` or `trend_score` (integer, -30 to +30)

Type and date are derived from filename (`YYYY-MM-DD-performance.md` / `YYYY-MM-DD-trend.md`).

### Required Sections

Scores, Trends, Summary.

Canonical templates are in `skills/assess/SKILL.md`.

---

## Decision Log: Format

Decisions are documented in a `## Decisions` section directly in the canvas file.

### Decision Entry Format

```markdown
### YYYY-MM-DD — [Continue|Pivot|Stop]

**Context:** [Summary of evidence and hypothesis status]
**Decision:** [Reasoning for the decision]
**Next Steps:** [What happens next]
```

### Pivot Mechanics

1. Original canvas folder moved to `archive/`
2. New canvas folder created in `explore/<new-slug>/` with `status: draft` and `pivot_from: <original-slug>`
3. Decision logged in the new canvas

### Stop Mechanics

1. Canvas folder moved to `archive/`
2. All content preserved inside the folder

---

## Folder Structure (Canvas-First)

```
.knvs/config.json
explore/                        — Canvases (draft and testing)
  <canvas-slug>/
    <canvas-slug>.md            — Canvas file
    hypotheses/                 — Hypotheses for this canvas
    experiments/                — Experiments for this canvas
    insights/                   — Insights for this canvas
    learning-cards/             — Learning Cards for this canvas
exploit/                        — Validated, scaling business models
  <canvas-slug>/
    <canvas-slug>.md            — Canvas file
    hypotheses/                 — Carried over from explore
    experiments/                — Carried over
    insights/                   — Carried over
    learning-cards/             — Carried over
    assessments/                — Performance & Trend assessments
archive/                        — Stopped/pivoted canvases
  <canvas-slug>/                — Everything preserved as-is
```

Canvas subfolders are created on-demand by the respective skills.

### Cross-Reference Convention

**Intra-canvas references** use canvas-relative paths (phase-agnostic):
- `canvas:` — slug only
- `hypothesis:`, `experiment:`, `source_experiment:`, `insights:` — relative to canvas folder

**External references** use root-relative paths or slugs:
- `source_research:` — root-relative
- `pivot_from:` — slug only (original always in `archive/`)

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/knvs:` prefix | `/knvs:start`, `/knvs:ideate`, `/knvs:hypothesize`, `/knvs:experiment`, `/knvs:capture`, `/knvs:learn`, `/knvs:card`, `/knvs:exploit`, `/knvs:assess` |
| Skill files | `<skill>/SKILL.md` | `skills/ideate/SKILL.md` |
| Configuration | `.knvs/` folder | `.knvs/config.json` |
| Phase folders | Short, unambiguous | `explore/`, `exploit/` |

---

## Markdown Generation

All generated files must work in Markdown viewers (Obsidian, VS Code, Notion)
AND be parseable by Claude Code.

---

## Tool-Agnostic: Manual Workflow Always Possible

All files are pure Markdown with standard frontmatter. Skills are helpers, not a prerequisite.
Users can create, move, and edit files manually. No proprietary formats, no tool-specific dependencies.

---

## References

Assessment dimensions are based on concepts from business model innovation literature
(Osterwalder, Pigneur, Smith, Etiemble). The BMC template follows the Osterwalder/Pigneur
Business Model Canvas (CC BY-SA 3.0).
