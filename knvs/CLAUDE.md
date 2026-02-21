# knvs: Plugin-Specific Rules

## Vision

**knvs** is a template for Innovation Managers based on the Strategyzer "Testing Business Ideas" process.

### Strict Rules

- **Development support only** for knvs (code gen, docs, tests, automation)
- **No user features** that Innovation Managers would use directly
- **No automatic deployments** without review

### Versioning

`CHANGELOG.md` is the single source of truth for the version. `plugin.json` is synced automatically via git pre-commit hook — never edit it manually.

| Change type | Version bump |
|-------------|-------------|
| New feature, Breaking change in Skills/Commands | Minor (`0.x.0`) |
| Bugfix, docs correction | Patch (`0.x.1`) |

Always add a `## [x.y.z] - YYYY-MM-DD` entry to `CHANGELOG.md` before committing.

### Warning Signal Check

**Before every task, ask:**
1. Does this help with the **development of knvs**?
2. Is it **not a user feature** for knvs end users?

**When in doubt:** Ask the developer IMMEDIATELY!

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
| Research Report | `[slug].md` | `machine-customers-verified.md` |
| Impact Atom | `[slug].md` | `vector-search-mandatory.md` |
| Network Canvas | `[entity-slug].md` | `vendor-corp.md` |

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

**Missing fields = invalid Canvas.** `/knvs:sync` checks for this.

**3-Tier Signal System:** Each BMC dimension carries signals at three levels:

| Tier | Signal | Visual |
|------|--------|--------|
| 1 | No signal | Nothing rendered |
| 2 | Open hypothesis (`status: hypothesis`, `canvas` matches) | `[!note]-` callout (subtle) |
| 3 | Confirmed MEDIUM or HIGH impact | `[!impact]` callout (full) |

LOW impacts are never rendered as individual callouts — they appear only in the overflow line.
HIGH ▲ Opportunities are always Tier 3 (filter is on severity, not direction).
See `skills/ideate/SKILL.md` for generation rules, same-driver merging, and callout format.

### Phase-Specific Extensions

In addition to the 9 core fields, each phase adds further required elements:

- **IDEATE:** Frontmatter `status`, `progress`, `created`, `updated`. Optional: `source_research`, `impact_cap` (default: 3, max callouts per BMC dimension). Sections: Notes, Next Steps
- **EXPLORE:** Frontmatter additionally `innovation_risk`, `potential_revenue`. Sections: Hypotheses, Experiments
- **EXPLOIT:** Frontmatter additionally `next_review`, `disruption_risk`, `revenue_score`. Sections: Reviews

Canonical templates for each phase are in the respective `skills/*/SKILL.md` under `## Canvas Template`.

---

## Research Report: Structure

Research reports in `research/` are PRE-IDEATE documents. They are NOT Business Model Canvases.

**Required fields (frontmatter):**
- `type: research`
- `title`
- `status` (hypothesis | draft | verified)
- `created`
- `verified` (YYYY-MM-DD, when status=verified)

**Optional fields:**
- `source_report` (reference to the original document)
- `vendor` (name of the external organization, when the report focuses on a specific vendor, partner, or competitor)

**Note:** `status: verified` can be set manually when the research report has already been
reviewed externally. The `verified` date field is also required for manual verification
(enter your own review date).

**Finalization workflow:** For AI-generated research drafts, the recommended path is:
1. External AI generates draft (`status: draft`)
2. Separate AI session creates a Maengelprotokoll (`research/<slug>-audit.md`)
3. `/knvs:finalize` incorporates audit findings and sets `status: verified`

See `skills/finalize/SKILL.md` for the editing protocol and audit format.

**Required Sections:**
- Summary
- Research Context (Problem Space, Market Context, Target Audience)
- Key Findings (with sources)
- Supporting Data
- Implications for Business Model
- Next Steps

**Optional Sections:**
- Limitations
- Bibliography

Research Reports are transformed via `/knvs:impact` into Impact Atoms and via `/knvs:ideate` into BMC Canvases.

### Research Report Template

```markdown
---
type: research
title: "[Research Topic]"
status: verified
created: YYYY-MM-DD
verified: YYYY-MM-DD
source_report: "[Original filename]"  # optional
---

# Research Report: [Title]

## Summary

[Overview of the research findings]

---

## Research Context

### Problem Space
[What problem does this research address?]

### Market Context
[Market size, trends, dynamics]

### Target Audience
[Who is affected by this problem?]

---

## Key Findings

### Finding 1: [Title]
[Content]

**Sources:**
- [Citation 1 with link]

---

## Supporting Data

### Market Statistics
[Statistics with sources]

### Competitive Landscape
[Competitive analysis]

### Technical Feasibility
[Technical assessments]

---

## Implications for Business Model

[Assessments of how this research could inform a business model]

Potential BMC fields:
- **Value Proposition:** [From problem space and findings]
- **Customer Segments:** [From target audience analysis]
- **Revenue Streams:** [From market context]

---

## Next Steps

- [ ] /knvs:impact to extract Impact Atoms
- [ ] /knvs:ideate for Business Model Canvas
```

### Hypothesis (Research Predecessor Stage)

Hypotheses are UNCONFIRMED CLAIMS that arise during the work.
They are research assignments, not facts.

**When to capture?**
When the user makes a claim that:
- Does not originate from verified research
- Influences business model decisions (e.g. severity, canvas design)
- Requires validation through external research

Every skill that encounters an unconfirmed market assumption offers
hypothesis capture. Users can also create hypotheses manually.

**Required fields (frontmatter):**
- `type: research`
- `title`
- `status: hypothesis`
- `created`
- `claim` (single line: the claim to be tested)
- `bmc_fields` (array of affected BMC fields, canonical English names)
- `canvas` (path to the originating canvas, e.g. `ideate/my-canvas.md`; auto-populated by `/knvs:ideate`)

**Optional fields:**
- `origin_impact` (path to the Impact Atom, in case of a severity dispute)

**Required Sections:**
- Claim (expands on the claim with context and nuance)
- Context (origin, affected canvas, severity decision)
- Research Assignment (checklist of concrete tasks)
- Deep Research Prompt (ready-to-copy prompt for Gemini/GPT, generated from claim + context)

Hypotheses contain NO findings, sources, or correction logs.

**Lifecycle:**

| Outcome | Action |
|---------|--------|
| Confirmed | File research in research/ with status: verified -> /knvs:impact -> delete hypothesis |
| Refuted | Correct severity in canvas -> delete hypothesis |
| Not relevant | Delete hypothesis |
| Not researched (>14 days) | /knvs:sync warns |

Hypotheses are working documents. Delete after research or rejection.
Canonical template is in `skills/ideate/SKILL.md` under `## Hypothesis Template`.

---

## Impact Atom: Structure

Impact Atoms in `impacts/` are PRE-IDEATE documents. They sit between Research and BMC.
Each Impact describes ONE specific pressure or opportunity for business models.

**Required fields (frontmatter):**
- `type: impact-atom` (fixed)
- `title`
- `tags` (array, categorization)
- `driver` (wiki link to research, or array for multiple sources)
- `bmc_fields` (array of affected BMC fields, canonical English names)
- `created` (YYYY-MM-DD)

**Required Sections:**
- Title with `[IMPACT]` prefix
- The scenario (What is happening?)
- Pressure on business models (bullet per affected BMC field)
- Source (wiki link to the Research Report)

**Rules:**
- Impact Atoms are immutable after creation
- **No severity in the atom.** Severity is assigned context-specifically during canvas embedding (`/knvs:ideate`).
- Filename: `<impact-slug>.md`
- BMC field names must exactly match the 9 canonical names
- Multi-driver: `driver` can be an array when the same insight comes from multiple sources

Canonical template is in `skills/impact/SKILL.md` under `## Impact Atom Template`.
Impact Atoms are embedded via `/knvs:ideate` as inline callouts in BMC Canvases and used via `/knvs:review` as a basis for challenges.

---

## Network Canvas: Structure

Network Canvases in `network/` map the business models of external parties (vendors,
partners, customers, competitors). They serve as the **home for indirect impacts** - impacts
that primarily affect the external model and only secondarily affect our canvases.

**Why separate?** When viewing our canvas, external impacts should not appear as
full `[!impact]` callouts. Instead, our canvas shows only a lightweight `[!vendor-risk]`
reference to the Network Canvas.

**Required fields (frontmatter):**
- `type: network-canvas` (fixed)
- `entity` (official name of the external organization)
- `relationship` (vendor | partner | customer | competitor | industry)
- `snapshot_date` (YYYY-MM-DD - when was this state current?)

**Optional fields:**
- `product` (product/platform if relevant, e.g. "STEP")
- `source_research` (wiki link to the Research Report that was the basis)

**BMC fields:** Only fill in the analytically relevant fields - no obligation to fill all 9.
Impact callouts are embedded as in our own canvas (`> [!impact]`), but describe
the pressure on the EXTERNAL model, not on ours.

**No lifecycle:** No IDEATE/EXPLORE/EXPLOIT phases, no `next_review`, no
`innovation_risk`. Network Canvases are analysis snapshots, not innovation objects.

**Staleness:** `/knvs:sync` warns when `snapshot_date` is older than 90 days.

### Network Canvas Template

```markdown
---
type: network-canvas
entity: "Vendor Corp AG"
product: "PlatformX"
relationship: vendor
snapshot_date: YYYY-MM-DD
source_research: "[[research/vendor-corp-analysis]]"  # optional
---

# Vendor Corp/PlatformX - Vendor Model Snapshot

## Value Proposition

> [!impact]- ▼ AI-powered ERP alternatives put VP under pressure (HIGH)
> **Driver:** [[research/vendor-corp-analysis]]
>
> GenAI-native competitors offer comparable ERP platform functionality...
>
> _Source: [[research/vendor-corp-analysis]]_

[What Vendor Corp offers - only known / relevant aspects]

## Key Resources
[What Vendor Corp has as a Key Resource]

## Revenue Streams
[How Vendor Corp earns - relevant to our Cost Structure]
```

### `[!vendor-risk]` Callout in Our Own Canvas

When our canvas references a vendor/partner documented in `network/`,
a `[!vendor-risk]` callout is inserted directly under the affected BMC field:

```markdown
## Key Partnerships

> [!vendor-risk]- Vendor Corp/PlatformX: Vendor currently under pressure ([[network/vendor-corp]])
> Current situation: 2 HIGH, 1 MEDIUM impacts on vendor model.
> Risk: Planning reliability in this area is limited.

**Vendor Corp/PlatformX** is our primary ERP platform provider...
```

`[!vendor-risk]` is not an Impact Atom - it is a reference indicator. The impacts
themselves live in the Network Canvas, not in `impacts/` and not directly in our canvas.

---

## Namespace Strategy

| Element | Format | Example |
|---------|--------|---------|
| Skills | `/knvs:` prefix | `/knvs:impact`, `/knvs:ideate`, `/knvs:start` |
| Skill files | `<skill>/SKILL.md` | `ideate/SKILL.md`, `start/SKILL.md` |
| Configuration | `.knvs/` folder | `.knvs/config.json` |
| Phase folders | Short, unambiguous | `research/`, `impacts/`, `ideate/`, `explore/`, `exploit/` |

### Conflict Avoidance

- No generic file names (`config.json`, `settings.md`)
- No overwriting of standard files
- Clear separation through prefixes/namespaces
- Phase folders are short but specific enough

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
- "AI-slop" style (excessive emoji use)

---

## Tool-Agnostic: Manual Workflow Always Possible

**Core principle:** The user must always be able to carry out the knvs process **manually without skills**. The chosen Markdown tool (Obsidian, VS Code, Notion, etc.) must play no role.

**Rules:**
- Do not build features that only work with specific tools
- All files are pure Markdown with standard frontmatter
- Skills are helpers, not a prerequisite
- Users can create, move, and edit files manually
- Folder structure and file format are self-explanatory

**Forbidden:**
- Obsidian-specific plugins or Dataview queries as core functionality
- Proprietary file formats
- Features that only work with Claude Code
- Dependencies on external APIs or services
