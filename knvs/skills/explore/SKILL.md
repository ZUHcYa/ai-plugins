---
name: "explore"
description: "Move canvas to EXPLORE phase with hypothesis generation"
---

# /knvs:explore

## Purpose

Moves a canvas from `ideate/` to `explore/` and generates hypotheses with test suggestions.

## When to Use

- When your idea research is complete and you're ready to validate your assumptions
- Transitioning from IDEATE to EXPLORE phase

## Workflow

```
User: /knvs:explore
Claude: Which idea do you want to explore?

        1. AI Bookkeeping for Freelancers [READY FOR EXPLORE]
        2. Fitness App for Seniors [WIP]

User: 1
Claude: Analyzing Business Model Canvas...

        I've identified 4 critical hypotheses from your canvas:

        ### Hypothesis 1: Customers will pay EUR 15-30/month
        **Recommended Test:** Pricing survey (50+ freelancers)

        ### Hypothesis 2: AI can categorize receipts accurately
        **Recommended Test:** Prototype with 100 sample receipts

        [...]

        Canvas moved: ideate/ai-bookkeeping.md -> explore/ai-bookkeeping.md
        Status: EXPLORE
        Hypotheses: 4 identified with test suggestions
```

## What the Skill Does

1. Lists all canvases in `ideate/` folder (prioritizes READY FOR EXPLORE)
2. User selects which idea to explore
3. Analyzes BMC for critical assumptions:
   - Value Proposition -> Desirability hypotheses
   - Revenue Streams -> Willingness to pay
   - Customer Segments -> Target market fit
   - Channels -> Reachability
   - Cost Structure -> Feasibility
4. Generates 3 test suggestions per hypothesis
5. **Moves file from `ideate/` to `explore/`**
6. Updates canvas status to EXPLORE
7. Adds hypotheses section with suggested tests

## Test Types

Based on Strategyzer methodology:

| Category | Test Types |
|----------|------------|
| **Discovery** | Interviews, Surveys, Desk Research |
| **Validation** | Landing Page, Smoke Test, Pre-orders |
| **Efficiency** | A/B Test, Prototype, Concierge MVP |

## Notes

- File is physically moved from `ideate/` to `explore/`
- Status in frontmatter changes from IDEATE to EXPLORE
- Progress field is removed (only used in IDEATE phase)
- Hypotheses are generated based on BMC content analysis

## Next Steps

After hypotheses are generated:
1. Choose a test method for each hypothesis
2. Run the experiments (surveys, landing pages, prototypes, etc.)
3. Document results in the canvas (VALIDATED / INVALIDATED)
4. When all critical hypotheses are validated: `/knvs:exploit`
5. View your portfolio overview with `/knvs:start`

---

## Canvas Template

Canonical template for EXPLORE canvases:

```markdown
---
status: EXPLORE
created: YYYY-MM-DD
updated: YYYY-MM-DD
innovation_risk: MEDIUM # LOW | MEDIUM | HIGH
potential_revenue: 5 # 1-10
---

# Business Model Canvas: [Name]

## Value Proposition

## Customer Segments

## Channels

## Customer Relationships

## Revenue Streams

## Key Resources

## Key Activities

## Key Partnerships

## Cost Structure

---

## Hypotheses

### 1. [Hypothesis]
- **Status:** PENDING | TESTING | VALIDATED | INVALIDATED
- **Result:**

**Suggested Tests:**
1. [Test A] - [Type: Interview/Survey/Landing Page/Prototype/etc.]
2. [Test B] - [Type]
3. [Test C] - [Type]

**Selected Test:**
- **Test:**
- **Success Criteria:**
- **Duration:**

---

### 2. [Hypothesis]
- **Status:** PENDING
- **Result:**

**Suggested Tests:**
1. [Test A] - [Type]
2. [Test B] - [Type]
3. [Test C] - [Type]

**Selected Test:**
- **Test:**
- **Success Criteria:**
- **Duration:**

---

## Experiments

| Experiment | Hypothesis | Status | Result |
|------------|------------|--------|--------|
| | | | |

## Next Steps
- [ ]
```

---

## Experiment Structure

When documenting experiments, use this structure:

```markdown
# Experiment: [Name]

**Canvas:** [Link]
**Hypothesis:** [What is being tested?]
**Status:** Planned | Running | Completed

## Goal
## Experiment Design
- Test method
- Participants/Sample
- Duration
- Success criteria

## Results
## Conclusion
- Hypothesis validated? Yes | No | Partially
- Next steps
```

