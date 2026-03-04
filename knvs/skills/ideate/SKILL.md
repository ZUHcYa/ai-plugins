---
name: "ideate"
description: "Create a new Business Model Canvas as draft in explore/"
---

# /knvs:ideate

## Purpose

Creates a new Business Model Canvas in the `explore/` folder with `status: draft`.

## When to Use

- When you have a new business idea and want to capture it systematically
- Starting the innovation lifecycle for a new concept
- When pivoting: creating a new BMC variant based on learnings

## Workflow

### From Scratch

```
User: /knvs:ideate
Claude: What's your idea? Describe your initial thoughts...
User: [Describes idea]
Claude: Business Model Canvas created: "[Title]"
        File: explore/[slug]/[slug].md
        Status: draft
```

### From Pivot

```
User: /knvs:ideate (during /knvs:card pivot)
Claude: Creating new BMC variant based on pivot decision...

        Original canvas: explore/original-idea/ (archived to archive/original-idea/)
        Pivot reason: [from decision log]

        Business Model Canvas created: "Original Idea v2"
        File: explore/original-idea-v2/original-idea-v2.md
        Status: draft
        Pivot from: original-idea
```

## What the Skill Does

1. Asks for idea description (or receives pivot context from `/knvs:card`)
2. Automatically extracts appropriate title
3. Creates canvas folder `explore/[slug]/` and file `explore/[slug]/[slug].md` — slug is kebab-case derived from the title.
   NEVER add a date prefix or sequential number.
   CORRECT: `explore/ai-bookkeeping-app/ai-bookkeeping-app.md`
   WRONG:   `explore/20260220-001-ai-bookkeeping-app/...`
4. Initializes BMC template with `status: draft`
5. If pivot: sets `pivot_from` frontmatter field
6. Adds initial thoughts as notes

## Notes

- Canvas folder is created as `explore/[slug]/`
- Canvas file is `explore/[slug]/[slug].md` — no date prefix, no sequential number
- Initial status is always `draft`
- When the BMC is complete, set `status: testing` to begin the validation loop
- Then run `/knvs:hypothesize` to extract hypotheses

## Next Steps

After creating your canvas:
1. Open the file in your Markdown editor (Obsidian, VS Code, etc.)
2. Fill out all BMC sections (Value Proposition, Customer Segments, etc.)
3. Research and document your assumptions
4. When ready to validate: set `status: testing`
5. Run `/knvs:hypothesize` to extract hypotheses

---

## Canvas Template

Canonical template for new draft canvases.

> **STRICT:** Use ONLY the frontmatter fields listed below — do NOT add fields beyond the optional ones listed. `risk: high` is the default for new canvases (untested = high risk). `revenue` is empty until the user provides an estimate.

```markdown
---
status: draft
risk: high
revenue:
created: YYYY-MM-DD
updated: YYYY-MM-DD
pivot_from: original   # Optional: slug only, original always in archive/. Delete otherwise.
---

# Business Model Canvas: [Name]

## Value Proposition
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

## Decisions
<!-- Decision log entries will be added here by /knvs:card -->

## Notes
<!-- Initial thoughts, research notes, open questions -->

## Next Steps
- [ ]

---

> © Strategyzer AG — strategyzer.com — CC BY-SA 3.0
```
