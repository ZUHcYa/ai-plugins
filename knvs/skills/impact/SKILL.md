---
name: "impact"
description: "Extract atomic impacts from research for BMC pressure analysis"
---

# /knvs:impact

## Purpose

Extracts atomic impacts (Impact Atoms) from research reports.
Each impact describes a specific pressure or opportunity for business models.
Impact Atoms are the bridge between research and Business Model Canvas.

## When to Use

- After loading a research report into `research/`
- Before BMC creation (`/knvs:ideate`)
- When systematically capturing all business model implications of a research

## Workflow

### Single Research Paper

```
User: /knvs:impact
Claude: Which verified research report would you like to analyze?

        Available reports (research/):
        1. Machine Customers Analysis [VERIFIED]
        2. AI Commerce Trends [VERIFIED]
        3. Fitness Market Study [DRAFT]
        4. Vendor X KI-Wandel [HYPOTHESIS] (not available)

User: 1
Claude: Analyzing research/machine-customers-verified.md...

        I identified 4 potential impacts:

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 1 of 4
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Title: Expectation: Semantic Vectors Instead of Images
        Tags: [tech-shift, ai-readiness, market-pressure]
        BMC Fields: Value Proposition, Key Resources

        The scenario:
        Future buyers (AI agents) ignore pure HTML and image data.
        They require vectors and structured attributes.

        Pressure on business models:
        * Value Prop: Systems that only deliver "visuals" become invisible.
        * Key Resources: Vector databases and Schema.org expertise become mandatory.

        -> Accept? [Y/n/edit]

User: Y
Claude: Impact 1 saved.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 2 of 4
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Title: Price comparison becomes algorithmic
        Tags: [market-pressure, pricing]
        BMC Fields: Revenue Streams, Channels
        [...]

        -> Accept? [Y/n/edit]

User: n
Claude: Impact 2 skipped.

        [... continuing for remaining impacts ...]

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUMMARY
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Analyzed: research/machine-customers-verified.md
        Proposed: 4
        Accepted: 3
        Skipped: 1

        Saved files:
        - impacts/machine-customers--vector-search-mandatory.md
        - impacts/machine-customers--schema-org-pflicht.md
        - impacts/machine-customers--agent-negotiation.md

        Next steps:
        - /knvs:impact for additional research reports
        - /knvs:ideate to create BMC with impact context
```

### All Unprocessed Reports

```
User: /knvs:impact (then: "analyze all")
Claude: Scanning research/ for reports without impacts...

        Unprocessed reports:
        1. AI Commerce Trends [VERIFIED] - 0 impacts

        Already processed reports:
        2. Machine Customers [VERIFIED] - 3 impacts

        Start with "AI Commerce Trends"?

User: Y
Claude: [... same batch-review flow as above ...]
```

---

## What the Skill Does

1. Reads `.knvs/config.json` for targetPath
2. Scans `research/` for reports with `status: verified` or `status: draft` (hypotheses excluded)
3. If no argument: lists available reports, user selects
4. If user says "all": scans `driver` fields of all impacts in `impacts/` and finds reports without corresponding atoms
5. Reads the selected research report fully
6. Identifies distinct impacts:
   - Each impact = one specific pressure OR opportunity
   - Must affect at least one BMC field
   - Must be derivable from the research content (no speculation)
7. Checks for duplicates: compares title and `bmc_fields` with existing atoms in `impacts/`
   - On match: shows existing impact and asks whether to merge (multi-driver) or skip
8. Presents each impact individually for confirmation:
   - Title, tags, BMC fields, scenario, pressure
   - User can: accept (Y), reject (n), or edit
   - On "edit": user can adjust tags, BMC fields
9. Writes confirmed impacts as individual .md files to `impacts/`
10. Shows summary at end

---

## Impact Detection

The skill searches the research report for:

| Source in Report | Impact Type |
|-------------------|------------|
| Key Findings | Direct technology or market pressure |
| Business Model Implications | Explicit BMC effects |
| Supporting Data / Statistics | Quantifiable market shifts |
| Limitations | Uncertainties that still create action pressure |

### Vendor and Partner Analysis

If a research report covers an external organization (vendor, partner, customer,
competitor), the extracted impacts land **in the Network Canvas of that external party**
(`network/<slug>.md`), not in `impacts/`.

```
Claude: This report is about "Vendor Corp AG" (Vendor).
        Impacts will be embedded in network/vendor-corp.md, not in impacts/.

        Impact 1: AI-powered alternatives put VP under pressure
        -> [!impact] callout in network/vendor-corp.md, section "## Value Proposition"
```

**Why?** Impacts on external models should appear in our canvas only as lightweight
`[!vendor-risk]` references, not as full impact callouts. The Network Canvas is the
home of these impacts.

**Detection rule:** If research has `vendor` field set, or title/content focuses on a
specific external organization, ask about Network Canvas:

```
Claude: This report analyzes "Vendor Corp AG".
        Does network/vendor-corp.md already exist?
        [Y] Embed impacts directly in Network Canvas
        [N] Create new Network Canvas, then embed impacts
        [A] Treat as regular Impact Atoms (impacts/)
```

**For `[N]` - Create Network Canvas:**

```
Claude: Create new Network Canvas for "Vendor Corp AG".

        Relationship to this organization?
        [1] vendor    - Supplier / Technology Partner
        [2] partner   - strategic partner
        [3] customer  - B2B customer
        [4] competitor - competitor
        [5] industry  - Industry / market segment

User: 1

Claude: Product/Platform name? (Enter for none)
User: PlatformX

Claude: Network Canvas created: network/vendor-corp.md
        -> Impacts will now be embedded as [!impact] callouts...
```

The created Network Canvas uses the template from `CLAUDE.md` with the BMC fields
identified during research as sections.

For `[A]`: normal impact extraction to `impacts/`, formulation stays generic
("A business model that uses [Vendor] as a partner, ...").

| Source in Report | Impact Type | Destination |
|---|---|---|
| Key findings about OWN market | Direct technology or market pressure | `impacts/` |
| Key findings about external vendor/partner | Pressure on their business model | `network/<slug>.md` |

### BMC Field Assignment

Each impact must be assigned to at least one of the 9 canonical BMC fields:
- Value Proposition
- Customer Segments
- Channels
- Customer Relationships
- Revenue Streams
- Key Resources
- Key Activities
- Key Partnerships
- Cost Structure

One impact can affect multiple fields. The assignment is presented to the user for confirmation.

For ambiguous findings that are more prediction than fact,
see "Hypothesis (Research Predecessor Stage)" in CLAUDE.md.

---

## Duplicate Detection

```
Claude: Similar impact already exists:

        Existing: impacts/machine-customers--vector-search-mandatory.md
        - "Expectation: Semantic Vectors Instead of Images"
        - Driver: [[research/machine-customers-verified]]

        New proposal from ai-commerce-trends:
        - "Vector search becomes standard requirement"
        - Driver: [[research/ai-commerce-trends-verified]]

        Options:
        [M] Merge: Add driver to existing impact
        [N] New: Save as separate impact
        [S] Skip: Skip
```

On **Merge** the `driver` field is extended to an array:

```yaml
driver:
  - "[[research/machine-customers-verified]]"
  - "[[research/ai-commerce-trends-verified]]"
```

---

## Edge Cases

### Research without clear impacts

```
Claude: Analysis complete.

        I could not derive any specific business model impacts.

        Possible reasons:
        - Report is too general / too vague
        - Findings are descriptive without normative action pressure

        Recommendation: Review report manually for implicit opportunities,
        or use /knvs:ideate directly with the research as source.
```

### No usable report available

```
Claude: No research reports found in research/.

        Options:
        - Create a file in research/ with status: verified or status: draft
        - Hypotheses (status: hypothesis) are not research reports and are not listed
```

---

## Notes

- Impact Atoms are NOT Business Model Canvases
- They are pre-ideate documents that sit between research and BMC
- Impacts are immutable after creation
- **Impact Atoms contain NO severity rating.** Severity is assigned during canvas embedding (`/knvs:ideate`) because it requires the context of a specific business model.
- Deletion/archiving is possible manually
- Wiki links use `[[research/filename]]` format (Obsidian-compatible)
- "Analyze all" is natural language, not a CLI flag

---

## Impact Atom Template

Canonical template for Impact Atoms:

```markdown
---
type: impact-atom
title: "[Short descriptive title]"
tags: [tag1, tag2]
driver: "[[research/filename]]"
bmc_fields:
  - Value Proposition
  - Key Resources
created: YYYY-MM-DD
---

# [IMPACT] Title

**The scenario:**
[What is happening / what is changing? Concrete and fact-based from the research.]

**Pressure on business models:**
* **[BMC Field 1]:** [Specific pressure or opportunity for this field]
* **[BMC Field 2]:** [Specific pressure or opportunity for this field]

**Source:** [[research/filename]] - [Relevant finding/section]
```

### Filename

Pattern: `impacts/<impact-slug>.md`

Examples:
- `impacts/vector-search-mandatory.md`
- `impacts/schema-org-pflicht.md`
- `impacts/algorithmic-pricing.md`

The research origin is in the `driver` frontmatter field, not in the filename.
