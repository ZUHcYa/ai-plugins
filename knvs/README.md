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
| IDEATE: Create Business Model Canvas                      |
|  - Capture your business idea systematically              |
|  - Fill in all 9 BMC dimensions                           |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| EXPLORE: Validate through the Loop                        |
|  - Extract D/F/V hypotheses from BMC                      |
|  - Prioritize by importance × evidence                    |
|  - Design and run experiments                             |
|  - Extract insights from results                          |
|  - Decide: Persevere, Pivot, or Kill                      |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| EXPLOIT: Scale validated model                            |
|  - Run and optimize the business                          |
|  - Conduct quarterly disruption reviews                   |
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

### 3. Move to Validation

```
/knvs:explore
```

### 4. Extract Hypotheses

```
/knvs:hypothesize
```

### 5. Design Experiments

```
/knvs:experiment
```

### 6. Extract Insights

```
/knvs:learn
```

### 7. Make a Decision

```
/knvs:decide
```

---

## Available Skills

### `/knvs:start` - Smart Entry Point
Setup (first run) or portfolio overview (after setup).

### `/knvs:ideate` - Create New Idea
Creates a new Business Model Canvas in the IDEATE phase.

### `/knvs:ideas` - List Open Ideas
Shows all canvases in IDEATE phase with progress status.

### `/knvs:explore` - Start Validation
Moves canvas from IDEATE to EXPLORE phase.

### `/knvs:hypothesize` - Extract Hypotheses
Analyzes BMC and extracts Desirability/Feasibility/Viability hypotheses.

### `/knvs:experiment` - Design Experiment
Creates a structured experiment to test a hypothesis.

### `/knvs:learn` - Extract Insights
Distills key learnings from completed experiments.

### `/knvs:decide` - Persevere/Pivot/Kill
Presents evidence dashboard and documents the decision.

### `/knvs:exploit` - Scale Business Model
Moves validated canvas from EXPLORE to EXPLOIT phase.

### `/knvs:review` - Disruption Review
Quarterly review of EXPLOIT canvases for risks and opportunities.

### `/knvs:sync` - Check Consistency
Scans all files for inconsistencies and broken references.

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
|   |- ideas/SKILL.md
|   |- explore/SKILL.md
|   |- hypothesize/SKILL.md
|   |- experiment/SKILL.md
|   |- learn/SKILL.md
|   |- decide/SKILL.md
|   |- exploit/SKILL.md
|   |- review/SKILL.md
|   |- sync/SKILL.md
|
|- ideate/                      # Ideas in research phase
|- explore/                     # Ideas being validated
|- exploit/                     # Validated & scaling
|- hypotheses/                  # Hypotheses per canvas
|   |- canvas-slug/
|- experiments/                 # Experiments per canvas
|   |- canvas-slug/
|- insights/                    # Insights per canvas
|   |- canvas-slug/
|- reviews/                     # Disruption review history
|   |- canvas-slug/
|- archive/                     # Killed/pivoted canvases
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
