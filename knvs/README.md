# knvs - Innovation Management with Claude Code

**Systematically develop, test, and validate business ideas.**

---

## What is knvs?

**knvs** is a Claude Code plugin for hypothesis-driven business model validation.
It helps you systematically validate business ideas through structured experimentation.

### Who is knvs for?

**Innovation Managers, Product Managers, Founders, and Intrapreneurs** who:
- Want to systematically validate new business ideas
- Work hypothesis-driven (not gut feeling)
- Use structured canvases (Business Model Canvas or custom)
- Want Claude Code as an intelligent sparring partner

---

## Installation

### Claude Code Plugin (Recommended)

**Step 1:** Add the marketplace
```
/plugin marketplace add ZUHcYa/ai-plugins
```

**Step 2:** Install the plugin
```
/plugin install knvs@ai-plugins
```

Done! The plugin is ready to use.

---

## The Innovation Loop

```
IDEATE → HYPOTHESIZE → PRIORITIZE → EXPERIMENT → LEARN → CARD (DECIDE)
Canvas    Extract        Confidence ×   Run tests    Extract   Learning Card
          hypotheses     Importance                  Insights  Continue/Pivot/Stop
```

```
+----------------------------------------------------------+
| EXPLORE: Create and validate Canvas                       |
|  - Create Canvas as draft (/knvs:ideate)                  |
|  - Fill in all content sections                           |
|  - Set status: testing when ready                         |
|  - Extract hypotheses from canvas                         |
|  - Prioritize by importance x confidence                  |
|  - Design and run experiments                             |
|  - Extract insights from results                          |
|  - Learning Card: conclude with Continue, Pivot, or Stop  |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| EXPLOIT: Scale validated model                            |
|  - Run and optimize the business                          |
|  - Performance & Trend assessments (/knvs:assess)         |
+----------------------------------------------------------+
```

---

## Quick Start

1. `/knvs:start` — Setup workspace
2. `/knvs:ideate` — Create your first canvas
3. Fill out canvas sections, set `status: testing`
4. `/knvs:hypothesize` — Extract hypotheses
5. `/knvs:experiment` — Design tests
6. `/knvs:capture` — Start experiment and document results
7. `/knvs:learn` — Extract insights
8. `/knvs:card` — Learning Card + Decision (Continue/Pivot/Stop)

---

## Available Skills

| Skill | Purpose |
|-------|---------|
| `/knvs:start` | Setup (first run) or portfolio overview with health checks |
| `/knvs:ideate` | Create a new Canvas as draft in `explore/` |
| `/knvs:hypothesize` | Extract hypotheses from canvas (default: Desirability/Feasibility/Viability for BMC) |
| `/knvs:experiment` | Design a structured experiment to test a hypothesis |
| `/knvs:capture` | Start experiment or document results |
| `/knvs:learn` | Extract insights from completed experiments |
| `/knvs:card` | Learning Card + Decision: [E] per experiment, [H] per hypothesis (Continue/Pivot/Stop) |
| `/knvs:exploit` | Move validated canvas from `explore/` to `exploit/` |
| `/knvs:assess` | Performance & Trend Assessment for EXPLOIT canvases (10 dimensions, -3 to +3) |

---

## File Structure

```
knvs/
├── explore/                    Canvas drafts and testing
│   └── canvas-slug/
│       ├── canvas-slug.md      Canvas file
│       ├── hypotheses/
│       ├── experiments/
│       ├── insights/
│       └── learning-cards/
├── exploit/                    Validated & scaling
│   └── canvas-slug/
│       ├── canvas-slug.md
│       └── assessments/        Performance & Trend
├── archive/                    Stopped/pivoted canvases
├── skills/                     Skill definitions
├── CLAUDE.md
├── README.md
└── CHANGELOG.md
```

---

## Manual Workflow (No Skills)

knvs works without Claude Code:

1. Create folders: `explore/`, `exploit/`, `archive/`
2. Create canvas: `explore/my-idea/my-idea.md` with `status: draft`
3. Fill out canvas sections, set `status: testing`
4. Create hypotheses, experiments, insights, learning cards as Markdown files
5. Add decisions to canvas `## Decisions` section

All files are standard Markdown — use any editor.

## Documentation

- [CHANGELOG.md](CHANGELOG.md) - Version history

---

## References

- Business Model Canvas: Osterwalder/Pigneur (CC BY-SA 3.0)
- Assessment dimensions: Osterwalder, Pigneur, Smith, Etiemble
