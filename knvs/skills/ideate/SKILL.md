---
name: "ideate"
description: "Create a new Business Model Canvas in the IDEATE phase"
---

# /knvs:ideate

## Purpose

Creates a new Business Model Canvas in the `ideate/` folder.

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
        File: ideate/[slug].md
        Status: IDEATE | Progress: WIP
```

### From Pivot

```
User: /knvs:ideate (during /knvs:decide pivot)
Claude: Creating new BMC variant based on pivot decision...

        Original canvas: explore/original-idea.md (archived)
        Pivot reason: [from decision log]

        Business Model Canvas created: "Original Idea v2"
        File: ideate/original-idea-v2.md
        Status: IDEATE | Progress: WIP
        Pivot from: explore/original-idea.md
```

## What the Skill Does

1. Asks for idea description (or receives pivot context from `/knvs:decide`)
2. Automatically extracts appropriate title
3. Creates file in `ideate/` folder as `[slug].md` — slug is kebab-case derived from the title.
   NEVER add a date prefix or sequential number.
   CORRECT: `ai-bookkeeping-app.md`
   WRONG:   `20260220-001-ai-bookkeeping-app.md`
4. Initializes BMC template in IDEATE phase (Progress: WIP)
5. If pivot: sets `pivot_from` frontmatter field
6. Adds initial thoughts as notes

## Progress States

IDEATE phase has two progress states:

| State | Description |
|-------|-------------|
| **WIP** | Work in progress, still researching/brainstorming |
| **READY FOR EXPLORE** | Research complete, ready to validate hypotheses |

> **STRICT:** Only these two values are valid for `progress`. Do NOT invent percentages or sub-states.

## Notes

- File is created in `ideate/` folder
- Filename: ONLY `[title-as-slug].md` — no date prefix, no sequential number
- Initial progress is always WIP
- Use `/knvs:explore` when ready to move to validation phase

## Next Steps

After creating your canvas:
1. Open the file in your Markdown editor (Obsidian, VS Code, etc.)
2. Fill out all BMC sections (Value Proposition, Customer Segments, etc.)
3. Research and document your assumptions
4. When research is complete, set `progress: READY FOR EXPLORE`
5. Move to validation with `/knvs:explore`

---

## Canvas Template

Canonical template for new IDEATE canvases.

> **STRICT:** Use ONLY the frontmatter fields listed below — do NOT add fields beyond the optional ones listed.

```markdown
---
status: IDEATE
progress: WIP
created: YYYY-MM-DD
updated: YYYY-MM-DD
pivot_from: explore/original.md   # Optional: only when created from pivot. Delete otherwise.
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
<!-- Decision log entries will be added here by /knvs:decide -->

## Notes
<!-- Initial thoughts, research notes, open questions -->

## Next Steps
- [ ]
```
