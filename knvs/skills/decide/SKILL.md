---
name: "decide"
description: "Make a Persevere/Pivot/Kill decision based on experiment evidence"
---

# /knvs:decide

## Purpose

Presents the current evidence state for an EXPLORE canvas and facilitates
a Persevere/Pivot/Kill decision. Documents the decision in the canvas.
**Only applicable to EXPLORE canvases.** EXPLOIT canvases do not use Persevere/Pivot/Kill decisions.

## When to Use

- After running experiments and extracting insights
- When you want to evaluate your EXPLORE business model based on evidence
- At regular checkpoints during the EXPLORE phase
- **NOT for EXPLOIT canvases** — these have their own mechanisms (Performance & Trend Assessments)

## Workflow

```
User: /knvs:decide
Claude: Which canvas needs a decision?

        1. AI Bookkeeping (explore/) - 3/5 hypotheses validated
        2. Fitness App (explore/) - 1/3 hypotheses validated

User: 1
Claude: ======================================
        DECISION DASHBOARD: AI Bookkeeping
        ======================================

        HYPOTHESES
        ----------
        ✅ receipt-categorization-need (Desirability, STRONG, VALIDATED)
        ✅ ai-accuracy-requirement (Feasibility, STRONG, VALIDATED)
        ✅ customers-will-pay-monthly (Viability, STRONG, VALIDATED)
        ❌ linkedin-channel-effectiveness (Desirability, STRONG, INVALIDATED)
        ⬜ cloud-hosting-costs (Viability, NONE, OPEN)

        EVIDENCE SUMMARY
        -----------------
        Validated: 3 (Desirability: 1, Feasibility: 1, Viability: 1)
        Invalidated: 1 (Desirability: 1)
        Open: 1 (Viability: 1)

        KEY INSIGHTS
        ------------
        1. Receipt categorization is a validated pain point (82% confirmation)
        2. AI accuracy meets threshold (90% categorization accuracy)
        3. Freelancers prefer annual billing (EUR 149/year)
        4. LinkedIn is not an effective channel for freelancers

        What is your decision?

        [P] PERSEVERE - Continue testing remaining hypotheses
        [V] PIVOT - Create new BMC variant based on learnings
        [K] KILL - Abandon this business model

User: P
Claude: PERSEVERE decision.

        What should happen next?

User: Test the cloud hosting costs hypothesis and find alternative
      channels to replace LinkedIn

Claude: Decision logged in canvas.

        ### 2026-02-22 — Persevere

        **Kontext:** 3/5 hypotheses validated. LinkedIn channel
        invalidated. Cloud hosting costs still untested.
        **Entscheidung:** Core value proposition and revenue model
        validated. Need alternative channel strategy and cost validation.
        **Naechste Schritte:**
        - Design experiment for cloud-hosting-costs hypothesis
        - Research alternative channels (SEO, tax advisor partnerships)

        Updated: explore/ai-bookkeeping.md

        Next steps:
        - /knvs:experiment for cloud-hosting-costs hypothesis
        - /knvs:hypothesize to add channel hypotheses
```

### Pivot Flow

```
User: V (PIVOT)
Claude: What aspect of the business model needs to change?

User: The channel strategy needs a complete rethink. Instead of
      direct-to-consumer, we should go B2B through tax advisors.

Claude: Creating pivot canvas...

        Original: explore/ai-bookkeeping.md → archived
        New canvas: explore/ai-bookkeeping-b2b.md
        Pivot from: explore/ai-bookkeeping.md

        Decision logged in both canvases.

        ### 2026-02-22 — Pivot

        **Kontext:** Direct channel (LinkedIn) invalidated.
        Core value prop validated but distribution needs rethinking.
        **Entscheidung:** Pivot from B2C to B2B distribution via
        tax advisors. Value proposition stays, channel changes.
        **Naechste Schritte:** Fill B2B canvas, rethink partnerships

        Next steps:
        - Edit explore/ai-bookkeeping-b2b.md with B2B model
        - Set status: testing when done
        - /knvs:hypothesize to extract hypotheses
```

### Kill Flow

```
User: K (KILL)
Claude: Are you sure? This will archive the canvas and all related data.

User: Yes, the market is too small
Claude: Canvas archived: explore/ai-bookkeeping.md → archive/ai-bookkeeping.md

        Decision logged:

        ### 2026-02-22 — Kill

        **Kontext:** Market validation showed insufficient demand.
        **Entscheidung:** Market too small for viable business model.
        **Naechste Schritte:** None — archived.

        Hypotheses, experiments, and insights remain in their folders
        for reference.
```

## What the Skill Does

1. Lists canvases in `explore/`
2. User selects canvas
3. Reads all hypotheses from `hypotheses/<canvas-slug>/`
4. Reads all insights from `insights/<canvas-slug>/`
5. Presents decision dashboard:
   - Hypothesis status overview (validated/invalidated/open)
   - Evidence summary by category (D/F/V)
   - Key insights list
6. User makes decision:
   - **Persevere:** Identifies next hypotheses to test. Canvas stays in `explore/`. Decision logged.
   - **Pivot:** Creates new canvas in `explore/` with `status: draft` and `pivot_from` reference. Original moves to `archive/`. Decision logged in both.
   - **Kill:** Moves canvas to `archive/`. Decision logged.
7. Adds decision entry to `## Decisions` section in canvas

## Decision Criteria Guidance

The skill does NOT make the decision — it presents evidence and the user decides.
However, it can suggest based on patterns:

| Pattern | Suggestion |
|---------|------------|
| All D/F/V validated | Consider `/knvs:exploit` |
| Core D validated, F/V open | Persevere — test remaining |
| Core D invalidated | Consider Pivot or Kill |
| V invalidated | Consider Pivot (pricing/revenue model change) |
| All hypotheses invalidated | Consider Kill |

## Notes

- Decisions are documented in the canvas `## Decisions` section
- Pivot creates a new canvas in `explore/` with `status: draft` — the old one is archived
- Kill archives the canvas but keeps hypotheses/experiments/insights
- The `archive/` folder is for reference, not deletion
- Multiple decisions can be logged over time (the section is append-only)
