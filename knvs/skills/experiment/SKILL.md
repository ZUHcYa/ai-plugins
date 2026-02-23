---
name: "experiment"
description: "Design and document an experiment to test a hypothesis"
---

# /knvs:experiment

## Purpose

Designs and documents an experiment to test a specific hypothesis.
Offers predefined experiment types from "Testing Business Ideas" and allows custom types.
**Only applicable to EXPLORE canvases.** EXPLOIT canvases do not use experiments.

## When to Use

- After extracting hypotheses with `/knvs:hypothesize`
- When you want to test a specific assumption in an EXPLORE canvas
- When designing a validation experiment
- **NOT for EXPLOIT canvases** — these have their own mechanisms (Disruption Reviews)

## Workflow

```
User: /knvs:experiment
Claude: Which canvas are you testing?

        1. AI Bookkeeping (explore/) - 5 hypotheses (2 high priority)
        2. Fitness App (explore/) - 3 hypotheses

User: 1
Claude: Priority hypotheses for AI Bookkeeping:

        HIGH IMPORTANCE + NO EVIDENCE:
        1. receipt-categorization-need (Desirability)
        2. ai-accuracy-requirement (Feasibility)

        MEDIUM/WEAK:
        3. customers-will-pay-monthly (Viability, WEAK evidence)
        4. linkedin-channel-effectiveness (Desirability)
        5. cloud-hosting-costs (Viability)

        Which hypothesis do you want to test?

User: 1
Claude: Hypothesis: "Freelancers need automated receipt categorization"
        Category: Desirability
        BMC Fields: Value Proposition, Customer Segments

        Suggested experiment types:

        DISCOVERY (cheap, fast):
        [1] Customer Interview - Talk to 5-10 freelancers
        [2] Online Survey - Survey 50+ freelancers
        [3] Data Analysis - Analyze existing market data

        VALIDATION (more investment):
        [4] Landing Page / Smoke Test - Measure sign-up interest
        [5] Paper Prototype - Show mockup, gather feedback

        [C] Custom experiment type

User: 2
Claude: Setting up Online Survey experiment...

        Title: "Receipt Categorization Needs Survey"
        Success Criteria: What would validate this hypothesis?

User: >70% of respondents say receipt categorization is a top-3 pain point
Claude: Duration?

User: 2 weeks
Claude: Experiment created:
        File: experiments/ai-bookkeeping/receipt-categorization-survey.md
        Status: DESIGNED
        Hypothesis updated: status → testing

        Next steps:
        1. Run the survey
        2. Document results in the experiment file
        3. Run /knvs:learn to extract insights
```

## What the Skill Does

1. Lists canvases in `explore/` with hypothesis counts
2. User selects canvas
3. Shows hypotheses sorted by priority (high importance + no evidence first)
4. User selects hypothesis to test
5. Suggests experiment types based on hypothesis category:
   - **Discovery experiments** (cheap, fast): Customer Interview, Online Survey, Landing Page / Smoke Test, Data Analysis, Paper Prototype
   - **Validation experiments** (more investment): Concierge MVP, Wizard of Oz, Single Feature MVP, Pre-Sale, Crowdfunding
   - **Custom**: User defines their own experiment type
6. User defines success criteria and duration
7. Creates experiment file in `experiments/<canvas-slug>/`
8. Updates hypothesis status to `testing`

## Experiment Types (Testing Business Ideas)

### Discovery Experiments

| Type | Best For | Effort |
|------|----------|--------|
| Customer Interview | Understanding needs, discovering problems | Low |
| Online Survey | Quantifying preferences, broad validation | Low |
| Landing Page / Smoke Test | Measuring interest without building product | Medium |
| Data Analysis | Validating market size, trends | Low |
| Paper Prototype | Testing usability concepts | Low |

### Validation Experiments

| Type | Best For | Effort |
|------|----------|--------|
| Concierge MVP | Delivering value manually to validate demand | Medium |
| Wizard of Oz | Simulating product with human backend | Medium |
| Single Feature MVP | Testing one core feature | High |
| Pre-Sale | Validating willingness to pay | Medium |
| Crowdfunding | Validating demand and pricing at scale | High |

## Notes

- Experiment files are created in `experiments/<canvas-slug>/`
- One experiment tests one hypothesis
- Multiple experiments can test the same hypothesis
- The skill does NOT run the experiment — it designs and documents it
- Results should be documented directly in the experiment file

---

## Experiment Template

Canonical template (also in CLAUDE.md):

```markdown
---
type: experiment
title: "[Experiment name]"
canvas: explore/canvas-slug.md
hypothesis: hypotheses/canvas-slug/hypothesis-slug.md
experiment_type: online-survey
status: designed
created: YYYY-MM-DD
success_criteria: ">70% identify receipt categorization as top-3 pain point"
duration: "2 weeks"
evidence_strength:
---

# Experiment: [Title]

## Setup

[What we are testing and how]

## Success Criteria

[Measurable outcomes that would validate/invalidate the hypothesis]

## Process

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Results

(to be filled after experiment completion)

## Conclusion

(to be filled after experiment completion)
```
