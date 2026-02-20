---
name: "ideate"
description: "Create a new Business Model Canvas in the IDEATE phase"
---

# /knvs:ideate

## Purpose

Creates a new Business Model Canvas in the `ideate/` folder.

## When to Use

- When you have a new business idea and want to capture it systematically
- When you have a research report and want to transform it into a Business Model Canvas
- Starting the innovation lifecycle for a new concept

## Workflow

### From Scratch

```
User: /knvs:ideate
Claude: What's your idea? Describe your initial thoughts...
User: [Describes idea]
Claude: Business Model Canvas created: "[Title]"
        File: ideate/[slug].md
        Status: IDEATE | Progress: WIP
```

### From Verified Research

```
User: /knvs:ideate
Claude: Do you want to base this on existing research? [Y/n]
User: Y
Claude: Available research:
        1. AI Bookkeeping Analysis [VERIFIED]
        2. Fitness Market Study [DRAFT]
        3. Vendor X KI-Wandel [HYPOTHESIS] (not available)

        Which research report?
User: 1
Claude: Creating Business Model Canvas from research...

        Extracted from research:
        - Value Proposition: [from research findings]
        - Customer Segments: [from target audience]
        - Revenue Streams: [from market analysis]

        Business Model Canvas created: "AI Bookkeeping"
        File: ideate/ai-bookkeeping.md
        Status: IDEATE | Progress: WIP
        Source: research/ai-bookkeeping-analysis-verified.md
```

## What the Skill Does

1. Checks if research reports exist in `research/` folder (verified or draft; hypotheses excluded)
2. If yes: asks if starting from scratch or from research
3. If from research: lists verified and draft reports, pre-fills BMC sections from findings
4. If from research: scans `impacts/` for atoms where `driver` matches the selected research
5. If impacts found: displays them grouped by BMC field before canvas creation
6. If from scratch: asks for idea description
7. Automatically extracts appropriate title
8. Creates file in `ideate/` folder as `[slug].md` — slug is kebab-case derived from the title.
   NEVER add a date prefix or sequential number.
   CORRECT: `ai-bookkeeping-app.md`
   WRONG:   `20260220-001-ai-bookkeeping-app.md`
9. Initializes BMC template in IDEATE phase (Progress: WIP)
10. Adds initial thoughts or research findings as notes
11. If impacts exist: generates inline impact callouts under affected BMC dimensions

## Impact-Aware Canvas Creation

When creating a BMC from research, the skill automatically surfaces related Impact Atoms.

### Process

1. User selects a research report
2. Skill scans `impacts/` for atoms where `driver` contains the selected research
3. Groups impacts by `bmc_fields`
4. Displays impact summary, then generates canvas with inline callouts:

```
Claude: Creating BMC from research/machine-customers-verified.md...

        3 Impact Atoms found for this research:
        - vector-search-mandatory -> Value Prop, Key Resources
        - agent-negotiation -> Value Prop
        - algorithmic-pricing -> Revenue Streams

        Assessing severity for this canvas...
        [Severity Assessment for each impact, see below]

        Generating canvas with inline impact callouts...

        Value Proposition: 2 impacts embedded (1x HIGH, 1x MEDIUM)
        Key Resources: 1 impact embedded (HIGH)
        Revenue Streams: 1 impact embedded (MEDIUM)
        Other dimensions: No impacts

        Canvas created: ideate/machine-customers.md
```

5. For each impact: assesses severity in context of THIS specific BMC (see Severity Assessment below)
6. Each affected BMC dimension gets inline `> [!impact]` callouts with field-specific reasoning and assessed severity

### Vendor-Risk Awareness

After canvas creation, the skill checks if any referenced vendor/partner in the canvas
has a corresponding Network Canvas in `network/`. If yes, a `[!vendor-risk]` callout
is generated under the relevant BMC field.

**Detection:** When Key Partnerships, Key Resources, or other fields mention an
organization that exists as `entity` in a `network/*.md` file.

**Process:**
1. Scans all BMC fields of the new canvas for known vendor/partner names
2. Matches against `network/*.md` files (`entity` field in frontmatter)
3a. On match: insert `[!vendor-risk]` callout under the affected field
3b. No match (org known, but no Network Canvas): offer to create one

**Callout format:**
```markdown
> [!vendor-risk]- [Entity]: [relationship] with open impacts ([[network/<slug>]])
> Current situation: [optional brief summary of vendor status]
```

**Example - Network Canvas exists:**
```
Claude: "Vendor Corp/PlatformX" found in Key Partnerships.
        Network Canvas found: network/vendor-corp.md (2 impacts)

        -> [!vendor-risk] callout inserted in Key Partnerships:

        > [!vendor-risk]- Vendor Corp/PlatformX: Vendor under pressure ([[network/vendor-corp]])
        > Current situation: 2 impacts on vendor model (1 HIGH, 1 MEDIUM).
```

**Example - Network Canvas missing:**
```
Claude: "Cloud Vendor" mentioned in Key Partnerships.
        No Network Canvas found for Cloud Vendor.

        Create Network Canvas?
        [Y] Create now (empty, no impacts)
        [N] Later - apply /knvs:impact to Cloud Vendor research
        [A] Ignore (no vendor tracking for Cloud Vendor)

User: Y

Claude: Relationship to Cloud Vendor?
        [1] vendor [2] partner [3] customer [4] competitor [5] industry
User: 1

Claude: Network Canvas created: network/cloud-vendor.md (empty)
        -> Tip: apply /knvs:impact to research about Cloud Vendor to embed impacts.
        -> [!vendor-risk] callout inserted in Key Partnerships (no impacts yet).
```

`[!vendor-risk]` is NOT an Impact Atom. It does NOT appear in `impacts/`.
The actual impacts live in the Network Canvas (`network/vendor-corp.md`).

### No Impacts Available

If no impacts exist for the selected research, the skill proceeds normally:

```
Claude: No Impact Atoms found for this research.
        Tip: use /knvs:impact to extract impacts before BMC creation.
        Proceeding without impact context...
```

---

## Severity & Direction Assessment

Impact Atoms contain **no severity and no direction**. Both are assigned here - in the context of the specific BMC.

### Why here and not in the impact?

An impact describes WHAT happens (fact). How severe it is — and whether it is a threat or opportunity — depends on the specific business model:
- "Algorithmic price comparison" is **High ▼ Threat** for a B2C price-comparison platform (core value prop undermined)
- The same impact is **Low ▲ Opportunity** for a B2B analytics SaaS (data signal to improve recommendations)

### Severity Criteria (BMC-context-specific)

| Severity | Criterion |
|----------|-----------|
| **High** | Existential threat for THIS business model. Core canvas elements directly affected. |
| **Medium** | Competitive disadvantage for THIS business model. Adaptation needed, but not total failure. |
| **Low** | Optimization potential for THIS business model. Nice-to-have, not business-critical. |

### Direction Criteria (BMC-context-specific)

| Symbol | Direction | Criterion |
|--------|-----------|-----------|
| **▼** | Threat | Impact exerts pressure — the business model must react or face disadvantage. |
| **▲** | Opportunity | Impact creates an opening — the business model could exploit it for advantage. |
| **▼▲** | Both | Ambivalent — threatens current model but simultaneously enables a strategic pivot. |

### Process

1. For each Impact Atom: analyze how it affects the specific BMC dimensions of this canvas
2. Consider: Value Proposition, Customer Segments, Revenue Streams of the canvas
3. Propose **severity + direction** together with reasoning
4. User confirms or changes

### Example

```
Claude: 3 Impact Atoms found for this research.
        Assessing severity & direction for this canvas (B2C price-comparison platform)...

        Impact 1: Algorithmic price comparison
        BMC Fields: Revenue Streams, Channels
        Analysis: Transparency is the core value prop. Algorithmic buyers
                  bypass the UI and thus the commission model.
        Proposed: HIGH ▼ Threat

        -> Accept? [Y/n/edit]

User: Y

Claude: Impact 2: Vector search becomes standard
        BMC Fields: Key Resources
        Analysis: Platform is based on structured product data,
                  not on free-text search. Vector expertise is nice-to-have,
                  but early adoption could extend platform reach.
        Proposed: LOW ▲ Opportunity

        -> Accept? [Y/n/edit]
```

### Severity/Direction Dispute: Hypothesis Capture

When the user rejects the severity or direction and their reasoning is based on
an unverified market assumption:

1. Identify the assumption and formulate it as a single line
2. Ask: "This is not a confirmed finding from the research.
   Should I capture a hypothesis as a research assignment?"
3. If yes: create file in `research/<slug>.md` with `status: hypothesis`
   (see Hypothesis Template below)
4. Apply the user's severity and direction in the canvas
5. Add backlink to the callout reasoning:
   `(Severity + Direction based on hypothesis: [[research/<slug>]])`

**Generating the Deep Research Prompt (Step 3):**

When creating the hypothesis file, generate the `## Deep Research Prompt` section:

- **Framework selection:** Porter's 5 Forces for competitive/market-position claims
  (e.g. competitor moves, pricing, substitutes); PESTLE for macro/environmental claims
  (e.g. regulation, technology shifts, economic cycles). Choose the most fitting forces/factors.
- **Scope IN:** Derive from the claim + `bmc_fields`. Name the specific entity, behavior,
  or trend being tested and the metrics that would confirm or refute it.
- **Scope OUT:** Exclude adjacent topics not covered by `bmc_fields`
  (e.g. if bmc_fields is [Key Partnerships], exclude general market trends).
- **Strategic implications:** List the `bmc_fields` values verbatim.

Only offer hypothesis capture when the reasoning contains an unconfirmed
market assumption - not when there is a disagreement about the analysis.

```
Claude: Impact 2: Vector search becomes standard
        Proposed: LOW ▲ Opportunity
        -> Accept? [Y/n/edit]

User:   n - HIGH ▼ Threat, because [Vendor] is missing the AI shift

Claude: This assessment is based on an unverified assumption:
        "[Vendor] is missing the AI shift"

        Should I capture a hypothesis as a research assignment?
        [Y] Capture hypothesis + apply severity & direction
        [N] Apply severity & direction without capture
        [A] Stay with LOW ▲ Opportunity

User:   Y

Claude: -> research/vendor-ki-shift.md created
        -> HIGH ▼ Threat applied (with hypothesis backlink in callout)
```

---

## Hypothesis Template

Canonical template for hypothesis files in `research/`:

````markdown
---
type: research
title: "Vendor X is missing the AI shift"
status: hypothesis
created: YYYY-MM-DD
claim: "Vendor X is missing the AI shift and is losing market relevance as a result"
bmc_fields:
  - Key Partnerships
  - Value Proposition
origin_impact: impacts/vector-search-standard.md
---

# Hypothesis: Vendor X is missing the AI shift

**Status:** HYPOTHESIS (unconfirmed - not a research finding)

## Claim

Vendor X is missing the AI shift and is losing market relevance as a result.
This would have direct effects on Key Partnerships and Value Proposition,
because [context and nuances beyond the claim].

## Context

**Created during:** /knvs:ideate severity assessment
**Canvas:** ideate/my-canvas.md
**Impact:** impacts/vector-search-standard.md
**Severity decision:** Set to HIGH (instead of proposed LOW)

## Research Assignment

- [ ] Review Vendor X product strategy and AI roadmap
- [ ] Gather industry analyst assessments
- [ ] Compare competitor positioning

## Deep Research Prompt

*Bereit zum Kopieren in Gemini/GPT für Deep Research*

```
Research this claim with engineering-grade rigor:

**Claim:** Vendor X is missing the AI shift and is losing market relevance as a result.

**Scope:**
- IN: Vendor X product roadmap, AI strategy, recent releases, market share trends, customer churn signals
- OUT: General AI market trends, unrelated competitors, internal business model of the researching company

**Analytical Framework:** Porter's 5 Forces (Competitive Rivalry + Threat of Substitutes)

**Source Requirements:**
- Primary sources only: Gartner/Forrester/IDC reports, SEC/annual filings, official product announcements, earnings calls
- No blogs, SEO articles, or opinion pieces
- Every claim must include publication date and source

**Contrarian View Required:**
Research arguments AGAINST this claim. What evidence exists that Vendor X is successfully adapting to the AI shift?

**Deliverable:** Structured research brief addressing:
1. Evidence FOR the claim (with sources)
2. Evidence AGAINST the claim (contrarian view)
3. Verdict: Confirmed / Refuted / Insufficient Evidence
4. Strategic implications for: Key Partnerships, Value Proposition
```
````

---

## Inline Impact Callout Format

When creating a BMC from research with related impacts, embed callouts directly under affected dimensions.

### Generation Rules

1. **For each BMC dimension (`##` heading):**
   - Check if any impact atom has this dimension in `bmc_fields` array
   - If yes: generate inline callout immediately after the `##` heading, BEFORE the comment hint and content

2. **Callout structure:**
   ```markdown
   > [!impact]- ▼ [Impact Title from frontmatter] ([SEVERITY])
   > **Driver:** [[research/source-slug from driver field]]
   >
   > [Field-specific reasoning from "Pressure on business models" bullet for this dimension]
   >
   > _Source: [[impacts/impact-filename]]_
   ```

   Direction symbol prefix: `▼` (Threat), `▲` (Opportunity), `▼▲` (Both)

3. **Multiple impacts per dimension:**
   - Stack vertically (separate callouts, blank line between)
   - Order by severity: HIGH → MEDIUM → LOW

4. **Dimensions without impacts:**
   - Leave unchanged (only comment hint, no placeholder)

### Field-Specific Reasoning Extraction

Extract the relevant bullet from impact atom's "Pressure on business models" section.

**Example impact atom body:**
```markdown
**Pressure on business models:**
* **Value Proposition:** Systems that only deliver "visuals" become invisible.
* **Key Resources:** Vector databases become mandatory.
```

**Generated under `## Value Proposition`:**
```markdown
> [!impact]- ▼ Semantic Vectors Instead of Images (HIGH)
> **Driver:** [[research/machine-customers-verified]]
>
> Systems that only deliver "visuals" become invisible.
>
> _Source: [[impacts/machine-customers--vector-search-mandatory]]_
```

**Generated under `## Key Resources`:**
```markdown
> [!impact]- ▼ Semantic Vectors Instead of Images (HIGH)
> **Driver:** [[research/machine-customers-verified]]
>
> Vector databases become mandatory.
>
> _Source: [[impacts/machine-customers--vector-search-mandatory]]_
```

### Edge Cases

**Missing field-specific reasoning:**
If impact atom lists a BMC field in `bmc_fields` but has no matching bullet under "Pressure on business models":
Fallback to "The scenario" text from the impact atom.

**Multi-driver impacts:**
Show all drivers comma-separated:
```markdown
> [!impact]- ▼ Title (HIGH)
> **Driver:** [[research/source1]], [[research/source2]]
```

---

## Progress States

IDEATE phase has two progress states:

| State | Description |
|-------|-------------|
| **WIP** | Work in progress, still researching/brainstorming |
| **READY FOR EXPLORE** | Research complete, ready to validate hypotheses |

> **STRICT:** Only these two values are valid for `progress`. Do NOT invent percentages or sub-states.
> CORRECT: `progress: WIP`  —  WRONG: `progress: WIP (30%)`

## Notes

- File is created in `ideate/` folder
- Filename: ONLY `[title-as-slug].md` — no date prefix, no sequential number, no number at all
- Initial progress is always WIP
- Use `/knvs:explore` when ready to move to validation phase

## Next Steps

After creating your canvas:
1. Open the file in your Markdown editor (Obsidian, VS Code, etc.)
2. Fill out all BMC sections (Value Proposition, Customer Segments, etc.)
3. Research and document your assumptions
4. When research is complete, set `progress: READY FOR EXPLORE`
5. Start validation with `/knvs:explore`

---

## Canvas Template

Canonical template for new IDEATE canvases.

> **STRICT:** Use ONLY the frontmatter fields listed below — do NOT add extra fields.
> Follow the structure exactly: do not rename sections, change heading levels, or add sections.

```markdown
---
status: IDEATE
progress: WIP
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_research: research/filename.md  # ONLY include when created from research. DELETE this line otherwise.
---

# Business Model Canvas: [Name]

## Value Proposition

<!-- Inline impact callouts appear here when created from research with impacts.
Example:

> [!impact]- Impact Title (HIGH)
> **Driver:** [[research/source-slug]]
>
> Field-specific reasoning explaining pressure on this BMC dimension.
>
> _Source: [[impacts/impact-slug]]_

Multiple impacts stack vertically, sorted by severity (HIGH > MEDIUM > LOW).
-->

<!-- What value do we deliver? What problem do we solve? -->

## Customer Segments
<!-- Who are we creating value for? -->

## Channels
<!-- How do we reach our customers? -->

## Customer Relationships
<!-- What type of relationship does each segment expect? -->

## Revenue Streams
<!-- What are customers willing to pay? -->

## Key Resources
<!-- What resources does our value proposition require? -->

## Key Activities
<!-- What activities does our value proposition require? -->

## Key Partnerships
<!-- Who are our key partners and suppliers? -->

## Cost Structure
<!-- What are the most important costs? -->

---

## Notes
<!-- Initial thoughts, research notes, open questions -->

## Next Steps
- [ ]
```
