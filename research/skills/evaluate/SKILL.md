---
name: "evaluate"
description: "Evaluate a research report or text for critical thinking quality: biases, logical fallacies, and evidence gaps"
---

# /research:evaluate

## Purpose

Analyzes a research report (or any text) for critical thinking quality.
Detects logical fallacies, cognitive biases, unbacked claims, and argument
structure weaknesses. Produces a structured evaluation report.

This is a **self-critique tool** — use it before sending a draft to external audit
to catch obvious issues early.

## When to Use

- Before external audit: pre-screen a draft for critical thinking issues
- Evaluating imported content: assess a third-party report's reasoning quality
- Learning: understand what makes an argument strong or weak

---

## Workflow

### Standard Flow

```
User: /research:evaluate research/machine-customers.md
Claude: Loading research/machine-customers.md...

        CRITICAL THINKING EVALUATION
        =============================
        Report: Machine Customers Analysis (draft)
        Sections analyzed: 7
        Claims identified: 18

        LOGICAL FALLACIES (3 found)
        ============================

        1. Appeal to Authority (Section: Key Findings > Finding 2)
           Claim: "According to Gartner, machine customers are inevitable"
           Issue: Uses Gartner's authority as sole justification
                  without examining the underlying evidence.
           Fix:   Add the data Gartner cites, or qualify as "Gartner projects..."

        2. Hasty Generalization (Section: Supporting Data)
           Claim: "Three companies have adopted this → the market is moving"
           Issue: Three cases do not establish a market trend.
           Fix:   Present as anecdotal evidence, add market-level data.

        3. False Cause (Section: Implications)
           Claim: "AI adoption increased, therefore machine customers grew"
           Issue: Correlation presented as causation without mechanism.
           Fix:   Explain the causal chain or qualify as correlation.

        BIAS INDICATORS (2 found)
        ==========================

        1. Confirmation Bias — overall report
           Pattern: All 8 sources support the thesis. No counter-evidence cited.
           Fix:     Search for and include dissenting views or limitations.

        2. Survivorship Bias — Supporting Data > Case Studies
           Pattern: Only successful adopters mentioned.
           Fix:     Include failed attempts or note the selection bias.

        UNBACKED CLAIMS (4 found)
        ==========================

        1. "The market will reach $50B by 2028" — no source cited
        2. "Most enterprises are evaluating machine customers" — "most" is unquantified
        3. "Implementation costs are declining rapidly" — no data
        4. "Customer satisfaction improves with automation" — no study cited

        ARGUMENT STRUCTURE
        ===================
        Toulmin Analysis:
        - Claims:     18 (identified)
        - With Data:  11 (61%)  <-- should be >80% for strong research
        - With Warrant: 6 (33%)  <-- reasoning connecting data to claim
        - With Qualifier: 3 (17%)  <-- "most", "likely", "under conditions X"
        - With Rebuttal: 0 (0%)  <-- no counter-arguments addressed

        Verdict: WEAK argument structure — many claims lack supporting
                 reasoning and no counter-arguments are addressed.

        EVALUATION SUMMARY
        ====================
        | Dimension | Score | Assessment |
        |-----------|-------|------------|
        | Logical Soundness | 6/10 | 3 fallacies found |
        | Bias Awareness | 4/10 | Confirmation bias pattern |
        | Evidence Quality | 5/10 | 4 unbacked claims, 61% with data |
        | Argument Structure | 3/10 | Missing warrants, no rebuttals |
        | Overall | 4.5/10 | Significant issues — revise before audit |

        Output saved: research/machine-customers-evaluation.md

        Next steps:
        1. Fix flagged issues in the draft
        2. Then proceed to external audit → /research:finalize
```

---

## What the Skill Does

1. Read `.research/config.json` for targetPath
2. Accept file path as argument, or list drafts for selection
3. Read the target file fully
4. **Claim extraction:** Identify all factual claims, statistics, and assertions
5. **Fallacy scan:** Check each claim and argument for logical fallacies
6. **Bias scan:** Analyze overall patterns for cognitive biases
7. **Unbacked claims:** Flag claims without cited evidence
8. **Argument structure:** Apply Toulmin model analysis
9. **Score:** Generate dimensional scores and overall assessment
10. **Save:** Write evaluation report as `research/<slug>-evaluation.md`
11. Present findings with specific fix recommendations

---

## Logical Fallacies Detected

The skill checks for the most common fallacies in research writing:

| Fallacy | Detection Pattern |
|---------|-------------------|
| **Appeal to Authority** | Claim justified solely by who said it, not evidence |
| **Hasty Generalization** | Few examples presented as universal trend |
| **False Cause** | Correlation stated as causation without mechanism |
| **Cherry Picking** | Selective data that supports thesis while ignoring contradictions |
| **Straw Man** | Competing views oversimplified before dismissal |
| **Ad Hominem** | Source dismissed based on who they are, not what they say |
| **False Dichotomy** | "Either X or Y" when other options exist |
| **Slippery Slope** | Chain of consequences assumed without justification |
| **Appeal to Novelty** | "New therefore better" without evidence |
| **Bandwagon** | "Everyone is doing it" as justification |

Only flag when confidence is HIGH. Borderline cases are noted but not scored.

---

## Bias Indicators Detected

| Bias | Pattern |
|------|---------|
| **Confirmation Bias** | All sources agree, no dissenting views included |
| **Selection Bias** | Sample of examples is not representative |
| **Survivorship Bias** | Only successful cases mentioned, failures absent |
| **Anchoring Bias** | First statistic heavily influences conclusions |
| **Recency Bias** | Recent data overweighted vs historical context |
| **Authority Bias** | Well-known sources accepted uncritically |

---

## Toulmin Argument Model

Every claim is analyzed for:

| Element | What it is | Example |
|---------|-----------|---------|
| **Claim** | The assertion being made | "Machine customers will grow 40% YoY" |
| **Data** | Evidence supporting the claim | "Gartner reports 35-45% CAGR" |
| **Warrant** | Reasoning connecting data to claim | "Because automated procurement reduces costs" |
| **Qualifier** | Conditions or limitations | "In enterprise B2B contexts" |
| **Rebuttal** | Counter-arguments addressed | "However, regulatory barriers may slow adoption" |

Strong research has >80% of claims backed by data, >50% with explicit warrants,
and at least some rebuttals addressing counter-arguments.

---

## Evaluation Report Format

```markdown
---
type: research-evaluation
target: "research/<slug>.md"
evaluator: "Claude / research:evaluate"
evaluation_date: YYYY-MM-DD
scores:
  logical_soundness: 6
  bias_awareness: 4
  evidence_quality: 5
  argument_structure: 3
  overall: 4.5
---

# Evaluation: [Research Title]

**Target:** research/<slug>.md
**Date:** YYYY-MM-DD

## Logical Fallacies

### 1. [Fallacy Name] — [Section]
**Claim:** "..."
**Issue:** ...
**Fix:** ...

## Bias Indicators

### 1. [Bias Name] — [Scope]
**Pattern:** ...
**Fix:** ...

## Unbacked Claims

1. "[Claim]" — [what's missing]

## Argument Structure (Toulmin)

| Metric | Count | Percentage |
|--------|-------|------------|
| Claims | N | — |
| With Data | N | X% |
| With Warrant | N | X% |
| With Qualifier | N | X% |
| With Rebuttal | N | X% |

## Summary

| Dimension | Score | Assessment |
|-----------|-------|------------|
| Logical Soundness | /10 | |
| Bias Awareness | /10 | |
| Evidence Quality | /10 | |
| Argument Structure | /10 | |
| Overall | /10 | |
```

---

## Edge Cases

### File not found

```
Claude: File not found: research/nonexistent.md

        Available reports:
        1. machine-customers.md (draft)
        2. saas-pricing.md (verified)

        Select a file to evaluate: [1-2]
```

### No claims found

```
Claude: The document contains no identifiable factual claims.
        This may be a template, outline, or non-research document.

        Evaluation not possible — nothing to analyze.
```

### Already verified report

```
Claude: research/machine-customers.md has status: verified.

        Evaluation is still useful for continuous improvement,
        but findings will not change the verified status.

        Proceed? [Y/n]
```

### Non-research file (no frontmatter)

```
Claude: This file has no research frontmatter.

        I can still evaluate it for critical thinking quality,
        but it will be treated as generic text (no status tracking).

        Proceed? [Y/n]
```

---

## Notes

- This is a self-critique tool, not a replacement for external audit
- The evaluation is heuristic — human judgment should review flagged items
- Scores are relative (6/10 means "room for improvement", not "failing")
- The skill does NOT modify the original file
- Evaluation files are kept for reference alongside the draft
- Tool-agnostic: evaluation report is standard Markdown
