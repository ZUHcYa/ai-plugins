# knvs - Innovation Management with Claude Code

**Systematically develop, test, and validate business ideas.**

---

## What is knvs?

**knvs** is a Claude Code plugin for operationalizing the **Strategyzer "Testing Business Ideas"** and **"The Invincible Company"** process. It helps you systematically validate business ideas through hypothesis-driven experimentation.

### Who is knvs for?

**Innovation Managers, Product Managers, Founders, and Intrapreneurs** who:
- Want to systematically validate new business ideas
- Work hypothesis-driven (not gut feeling)
- Use the Business Model Canvas professionally
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

## The Strategyzer Loop

```
IDEATE → HYPOTHESIZE → PRIORITIZE → EXPERIMENT → LEARN → DECIDE
  BMC      D/F/V      Evidence ×     Run tests    Extract   Persevere
                      Importance                  Insights  Pivot / Kill
```

```
+----------------------------------------------------------+
| EXPLORE: Create and validate Business Model Canvas        |
|  - Create BMC as draft (/knvs:ideate)                     |
|  - Fill in all 9 BMC dimensions                           |
|  - Set status: testing when ready                         |
|  - Extract D/F/V hypotheses from BMC                      |
|  - Prioritize by importance x evidence                    |
|  - Design and run experiments                             |
|  - Extract insights from results                          |
|  - Decide: Persevere, Pivot, or Kill                      |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| EXPLOIT: Scale validated model                            |
|  - Run and optimize the business                          |
|  - Performance & Trend assessments (/knvs:assess)         |
|  - Conduct quarterly disruption reviews (/knvs:review)    |
+----------------------------------------------------------+
```

---

## Getting Started

**Want a quick overview?** See the [Quick-Start Guide](QUICKSTART.md).

### 1. Setup

```
/knvs:start
```

### 2. Create Your First Business Idea

```
/knvs:ideate
```

### 3. Fill Out BMC and Set Status

Fill out all 9 BMC sections, then set `status: testing` in frontmatter.

### 4. Extract Hypotheses

```
/knvs:hypothesize
```

### 5. Design Experiments

```
/knvs:experiment
```

### 6. Start Experiment and Document Results

```
/knvs:capture
```

### 7. Extract Insights

```
/knvs:learn
```

### 8. Make a Decision

```
/knvs:decide
```

---

## Available Skills

### `/knvs:start` - Smart Entry Point
Setup (first run) or portfolio overview with health checks (after setup).

### `/knvs:ideate` - Create New Idea
Creates a new Business Model Canvas as draft in `explore/`.

### `/knvs:hypothesize` - Extract Hypotheses
Analyzes BMC and extracts Desirability/Feasibility/Viability hypotheses.

### `/knvs:experiment` - Design Experiment
Creates a structured experiment to test a hypothesis.

### `/knvs:capture` - Start or Complete Experiment
Starts a designed experiment (designed → running) or documents results after execution (running → completed).

### `/knvs:learn` - Extract Insights
Distills key learnings from completed experiments.

### `/knvs:decide` - Persevere/Pivot/Kill
Presents evidence dashboard and documents the decision.

### `/knvs:exploit` - Scale Business Model
Moves validated canvas from `explore/` to `exploit/`.

### `/knvs:assess` - Performance & Trend Assessment
Scores EXPLOIT canvases on 10 dimensions (-3 to +3). Performance (current snapshot) or Trend (future projection).

### `/knvs:review` - Disruption Review
Quarterly review of EXPLOIT canvases for risks and opportunities.

---

## File Structure

```
knvs/
|- .claude-plugin/
|   |- plugin.json
|
|- skills/
|   |- start/SKILL.md
|   |- ideate/SKILL.md
|   |- hypothesize/SKILL.md
|   |- experiment/SKILL.md
|   |- capture/SKILL.md
|   |- learn/SKILL.md
|   |- decide/SKILL.md
|   |- exploit/SKILL.md
|   |- assess/SKILL.md
|   |- review/SKILL.md
|
|- explore/                    # BMCs (draft and testing)
|- exploit/                    # Validated & scaling
|- hypotheses/                 # Hypotheses per canvas
|   |- canvas-slug/
|- experiments/                # Experiments per canvas
|   |- canvas-slug/
|- insights/                   # Insights per canvas
|   |- canvas-slug/
|- reviews/                    # Disruption review history
|   |- canvas-slug/
|- assessments/                # Performance & Trend assessments
|   |- canvas-slug/
|- archive/                    # Killed/pivoted canvases
|
|- CLAUDE.md
|- README.md
|- QUICKSTART.md
|- STRUCTURE.md
|- CHANGELOG.md
```

---

## Markdown Format

All content is Markdown — readable by humans and machines:
- Obsidian, Notion, VS Code (humans)
- Claude Code (AI sparring partner)
- Git-friendly, platform-independent

---

## Resources

### Strategyzer Methodology
- "Testing Business Ideas" by David Bland & Alex Osterwalder
- "The Invincible Company" by Alex Osterwalder et al.

---

**Good luck developing and validating your business ideas!**
