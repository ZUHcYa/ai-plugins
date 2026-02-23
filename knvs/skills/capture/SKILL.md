---
name: "capture"
description: "Start an experiment or document results of a completed experiment"
---

# /knvs:capture

## Purpose

Manages experiment lifecycle after design: starting experiments and documenting results.
Bridges the gap between `/knvs:experiment` (design) and `/knvs:learn` (insights).
**Only applicable to EXPLORE canvases.**

## When to Use

- After designing an experiment with `/knvs:experiment` — to start it
- After running an experiment in the real world — to document results
- Before running `/knvs:learn` — results must be documented first
- **NOT for EXPLOIT canvases**

## Workflow

### Entry Point

```
User: /knvs:capture
Claude: (checks for designed and running experiments)

        CASE A — Only designed experiments exist:
        → Goes to "Start Experiment" flow

        CASE B — Only running experiments exist:
        → Goes to "Document Results" flow

        CASE C — Both exist:
        → What would you like to do?
           [S] Start an experiment (designed → running)
           [D] Document results (running → completed)
```

### Mode A: Start Experiment (designed → running)

```
User: /knvs:capture
Claude: Experiments ready to start:

        AI Bookkeeping (explore/):
        1. Receipt Categorization Needs Survey [DESIGNED]
           Hypothesis: receipt-categorization-need
           Type: Online Survey | Duration: 2 weeks

        2. Pricing Willingness Test [DESIGNED]
           Hypothesis: customers-will-pay-monthly
           Type: Pre-Sale | Duration: 4 weeks

        Which experiment do you want to start?

User: 1
Claude: Starting experiment: Receipt Categorization Needs Survey

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        EXPERIMENT CONTEXT
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Hypothesis: "Freelancers need automated receipt categorization"
        Category: Desirability
        Type: Online Survey
        Duration: 2 weeks

        Success Criteria:
        >70% of respondents say receipt categorization is a top-3 pain point

        Process:
        1. Create survey with categorization questions
        2. Distribute via freelancer communities
        3. Collect responses over 2 weeks
        4. Analyze results

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        Updated: status → running

        When you have results, come back with /knvs:capture
        to document them.
```

### Mode B: Document Results (running → completed)

```
User: /knvs:capture
Claude: Running experiments with pending results:

        AI Bookkeeping (explore/):
        1. Receipt Categorization Needs Survey [RUNNING]
           Hypothesis: receipt-categorization-need
           Type: Online Survey | Started: 2026-02-10

        Which experiment has results?

User: 1
Claude: Documenting results for: Receipt Categorization Needs Survey

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        SUCCESS CRITERIA (for reference)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        >70% of respondents say receipt categorization is a top-3 pain point

        What were the raw results / observations?

User: 82 of 100 respondents (82%) ranked receipt categorization
      as a top-3 pain point. 45% ranked it #1. Strongest signal
      among freelancers with revenue > EUR 50k.

Claude: Results captured. Now the conclusion:
        Did the experiment meet the success criteria?

        [S] Success — criteria met
        [F] Failure — criteria not met
        [I] Inconclusive — unclear or mixed results

User: S
Claude: How strong is the evidence from this experiment?

        [W] Weak — qualitative data, small sample, stated preferences,
            biased sample, or indirect indicators
        [S] Strong — quantitative data, large representative sample,
            actual behavior, or direct measurement

User: S
Claude: ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        EXPERIMENT COMPLETED
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Experiment: Receipt Categorization Needs Survey
        Result: SUCCESS
        Evidence Strength: STRONG

        Updated:
        - status: running → completed
        - completed: 2026-02-23
        - result: success
        - evidence_strength: strong
        - ## Results section filled
        - ## Conclusion section filled

        Next step: /knvs:learn to extract insights
        and update hypothesis confidence level.
```

## What the Skill Does

### Mode A: Start Experiment

1. Lists experiments with `status: designed`, grouped by canvas
2. User selects experiment
3. Shows experiment context: hypothesis, success criteria, process steps
4. Updates experiment: `status: running`
5. Points user to come back with `/knvs:capture` after execution

### Mode B: Document Results

1. Lists experiments with `status: running`, grouped by canvas
2. User selects experiment
3. Shows success criteria as reference
4. Collects from user:
   - **Raw results / observations** → fills `## Results`
   - **Conclusion** → success / failure / inconclusive
   - **Evidence strength** → weak / strong
5. Updates experiment file:
   - `status: completed`
   - `completed: YYYY-MM-DD`
   - `result: success | failure | inconclusive`
   - `evidence_strength: weak | strong`
   - Fills `## Results` with user-provided data
   - Fills `## Conclusion` with structured summary
6. Points to `/knvs:learn` as next step

## Evidence Strength

Evidence strength reflects the quality of the data collected, independent of the result.

| Strength | Indicators |
|----------|-----------|
| **weak** | Qualitative data, small sample, stated preferences, biased sample, indirect indicators |
| **strong** | Quantitative data, large representative sample, actual behavior, direct measurement |

**Examples:**

| Experiment Type | Typical Evidence Strength |
|----------------|--------------------------|
| Customer Interview (5 people) | weak |
| Online Survey (100+ respondents) | strong |
| Landing Page / Smoke Test | weak (interest) to strong (sign-ups) |
| Pre-Sale (actual payment) | strong |
| Data Analysis (market data) | weak (secondary data) |
| Concierge MVP | strong (real usage) |

The final assessment is always made by the user — these are guidelines, not rules.

## Edge Cases

### No experiments in designed or running state

```
Claude: No experiments to start or document.

        - Use /knvs:experiment to design a new experiment
        - Or check experiments/ for completed ones
```

### Experiment already has results filled

```
Claude: Experiment "Receipt Categorization Survey" already has results
        documented (status: completed).

        Use /knvs:learn to extract insights from this experiment.
```

## Notes

- This skill does NOT run the experiment — it tracks the lifecycle
- The user performs the experiment in the real world
- Results must come from the user — the skill does NOT generate data
- Evidence strength is a property of the experiment methodology, not the result
- One experiment produces one evidence_strength value
- Multiple experiments can test the same hypothesis with different strengths
