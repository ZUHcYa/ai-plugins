# knvs Quick-Start Guide

Get started with knvs in 5 minutes.

---

## Installation

### Option A: Claude Code Plugin (Recommended)

**Step 1:** Add the marketplace
```
/plugin marketplace add ZUHcYa/ai-plugins
```

**Step 2:** Install the plugin
```
/plugin install knvs@ai-plugins
```

**Step 3:** Run the setup
```
/knvs:start
```

### Option B: Manual Setup

1. Create folders: `explore/`, `exploit/`, `hypotheses/`, `experiments/`, `insights/`, `learning-cards/`, `assessments/`, `archive/`
2. Create `.knvs/config.json` with `{ "targetPath": "./" }`

---

## 1. Setup

```
/knvs:start
```

On first run, knvs automatically sets up your workspace:

```
Created:
├── explore/
├── exploit/
├── hypotheses/
├── experiments/
├── insights/
├── learning-cards/
├── assessments/
├── archive/
└── .knvs/config.json

Ready! Run /knvs:ideate to capture your first business idea.
```

---

## 2. Capture Your First Idea

```
/knvs:ideate
```

Describe your idea. Claude creates a Business Model Canvas as draft in `explore/`.

**Then:** Open the file and fill out all BMC sections.

---

## 3. Start Validation

When your canvas is complete, set `status: testing` in frontmatter.

---

## 4. Extract Hypotheses

```
/knvs:hypothesize
```

Claude analyzes your BMC and extracts testable hypotheses:
- **Desirability** — Do customers want this?
- **Feasibility** — Can we build this?
- **Viability** — Is this financially sustainable?

Each hypothesis is prioritized by importance x evidence.

---

## 5. Design Experiments

```
/knvs:experiment
```

Pick a high-priority hypothesis and design an experiment:
- Customer Interview, Survey, Landing Page, Prototype, etc.
- Define success criteria and duration

---

## 6. Start Experiment and Document Results

```
/knvs:capture
```

Start a designed experiment (designed → running). After running it in the real world, come back to document results (running → completed) with evidence strength assessment.

---

## 7. Extract Insights

After documenting results:

```
/knvs:learn
```

Claude extracts key learnings and updates hypothesis evidence levels.

---

## 8. Create Learning Card (Optional)

```
/knvs:card
```

Synthesize your experiment into a standalone Learning Card — the classic "Testing Business Ideas" format:
- **We believed that...** (hypothesis)
- **We observed...** (experiment results)
- **From that we learned...** (insights)
- **Therefore we will...** (next action)

---

## 9. Make a Decision

```
/knvs:decide
```

Based on your evidence: **Persevere**, **Pivot**, or **Kill**.

---

## The Complete Lifecycle

```
/knvs:start → /knvs:ideate → fill canvas → set status: testing → /knvs:hypothesize
     |              |              |              |                       |
   Setup       Create BMC      Research      Begin loop           Extract D/F/V

→ /knvs:experiment → /knvs:capture → /knvs:learn → /knvs:card → /knvs:decide → /knvs:exploit
        |                  |              |            |             |              |
    Design test     Start & document   Insights   Learning      Persevere/      Scale
                      results                      Card          Pivot/Kill
```

---

## Check Your Status

Run `/knvs:start` anytime to see your portfolio and suggested actions.

---

## Helpful Skills

| Skill | When to use |
|-------|-------------|
| `/knvs:start` | Setup or portfolio overview |
| `/knvs:ideate` | Create a new BMC as draft |
| `/knvs:hypothesize` | Extract hypotheses from BMC |
| `/knvs:experiment` | Design a validation test |
| `/knvs:capture` | Start experiment or document results |
| `/knvs:learn` | Extract insights from results |
| `/knvs:card` | Create Learning Card from experiment |
| `/knvs:decide` | Persevere / Pivot / Kill |
| `/knvs:exploit` | Move validated canvas to scaling |
| `/knvs:assess` | Performance & Trend assessment for EXPLOIT |

---

## Manual Workflow (No Skills)

knvs works without Claude Code:

1. Create folders: `explore/`, `exploit/`, `hypotheses/`, `experiments/`, `insights/`, `learning-cards/`, `assessments/`, `archive/`
2. Create file: `explore/my-idea.md` with `status: draft` frontmatter
3. Fill out BMC sections
4. Set `status: testing`, create hypothesis files in `hypotheses/my-idea/`
5. Create experiment files in `experiments/my-idea/`
6. Document results, create insight files in `insights/my-idea/`
7. Create learning card files in `learning-cards/my-idea/` (optional)
8. Add decision to canvas `## Decisions` section

All files are standard Markdown — use any editor.
