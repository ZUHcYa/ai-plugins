# knvs - Innovation Management with Claude Code

**Systematically develop, test, and validate business ideas.**

---

## What is knvs?

**knvs** is a Claude Code plugin for operationalizing the **Strategyzer "Testing Business Ideas"** process. It helps you systematically develop, test, and validate business ideas - supported by Claude Code.

### Who is knvs for?

**Innovation Managers, Product Managers, Founders, and Intrapreneurs** who:
- Want to systematically validate new business ideas
- Want to professionally use Business Model Canvas
- Work hypothesis-driven (not gut feeling)
- Work with Claude Code as an intelligent sparring partner

---

## Installation

### Option 1: Claude Code Plugin (Recommended)

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

## The Testing Business Ideas Process

```
Research -> Impacts -> Business Model Canvas -> Hypotheses -> Validation -> Exploit

+----------------------------------------------------------+
| IMPACT: Extract atomic impacts from research              |
|  - Identify specific pressures on business models         |
|  - Map impacts to BMC fields                              |
|  - Create standalone impact atoms (facts, no severity)    |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| IDEATE: Develop new ideas                                 |
|  - Create Business Model Canvas                           |
|  - Assess impact severity in BMC context                  |
|  - Use verified research and impacts as foundation        |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| EXPLORE: Validate hypotheses                              |
|  - Identify critical assumptions                          |
|  - Design and run experiments                             |
|  - Make data-driven decisions                             |
+----------------------------------------------------------+
                           |
+----------------------------------------------------------+
| EXPLOIT: Scale validated model                            |
|  - Continuously optimize                                  |
|  - Conduct disruption reviews                             |
|  - Ensure competitiveness                                 |
+----------------------------------------------------------+
```

---

## Getting Started

**Want a quick overview?** See the [Quick-Start Guide](QUICKSTART.md) for a 5-minute introduction.

### 1. Initial Setup

Just run `/knvs:start` - it handles everything automatically:

```
/knvs:start
```

On first run, Claude sets up your workspace. On subsequent runs, it shows your portfolio status.

**Example:**

```
User: /knvs:start
Claude: Welcome to knvs!

        Install here (./)? [Y/n]

User: Y
Claude: Setting up your innovation workspace...

        Created:
        ├── research/
        ├── impacts/
        ├── ideate/
        ├── explore/
        ├── exploit/
        ├── reviews/
        └── .knvs/config.json

        Ready! Run /knvs:impact or /knvs:ideate to get started.
```

### 2. Start a New Business Idea

Use the `/knvs:ideate` skill to capture your first idea:

```
/knvs:ideate
```

Claude asks about your idea and automatically creates a Business Model Canvas in the IDEATE phase.

**Example:**

```
User: /knvs:ideate
Claude: What's your idea? Describe your initial thoughts...
User: I want to develop an app that helps freelancers
      automate their bookkeeping with AI...
Claude: Business Model Canvas created: "AI-Powered Bookkeeping for Freelancers"
        File: ideate/ai-bookkeeping.md
        Status: IDEATE | Progress: WIP
```

### 3. Fill in the Canvas

Open the created Markdown file and systematically fill in all areas:
- Value Proposition
- Customer Segments
- Channels
- Revenue Streams
- Key Resources
- Key Partnerships
- Key Activities
- Cost Structure

### 4. Work with Claude

Claude Code supports you with:
- Structuring and formatting Business Model Canvas
- Identifying and prioritizing hypotheses
- Designing experiments
- Conducting disruption reviews
- Maintaining documentation

---

## Available Skills

### `/knvs:impact` - Extract Impact Atoms
Extracts atomic impacts from research reports. Each impact describes one specific pressure or opportunity for business models, mapped to affected BMC fields.

**When to use:** After placing a research report in `research/`, before creating a BMC with `/knvs:ideate`.

### `/knvs:start` - Smart Entry Point
The only command you need to remember. Automatically detects context:
- **First run:** Sets up folder structure and config
- **After setup:** Shows portfolio status, priorities, and suggested actions

**When to use:** Always start here. First time? Setup runs automatically. Returning? See your status.

### `/knvs:ideate` - Start New Idea
Creates a new Business Model Canvas in the IDEATE phase with automatic title generation.

**When to use:** When you have a new business idea and want to capture it systematically.

### `/knvs:ideas` - List Open Ideas
Shows all canvases in IDEATE phase with their progress status (WIP or READY FOR EXPLORE).

**When to use:** When you want to see all your work-in-progress ideas at a glance.

### `/knvs:explore` - Start Validation Phase
Converts an IDEATE canvas to EXPLORE phase. Analyzes your Business Model Canvas and generates critical hypotheses with test suggestions.

**When to use:** When your idea research is complete and you're ready to validate your assumptions.

### `/knvs:exploit` - Transition to Scaling
Moves a validated canvas from EXPLORE to EXPLOIT phase. Checks hypothesis validation status.

**When to use:** When all critical hypotheses have been validated and you're ready to scale.

### `/knvs:review` - Disruption Review
Reviews an EXPLOIT canvas for risks, opportunities, and market changes. Claude analyzes your business model and provides insights.

**When to use:** Quarterly reviews of running businesses.

### `/knvs:sync` - Check for Changes
Reviews canvas changes and flags inconsistencies on demand.

- Detects modified, new, and deleted canvas files
- Checks for status/folder mismatches
- Warns about stale ideas and overdue reviews
- Offers to fix detected issues

**When to use:** After editing files in Obsidian, to ensure everything is consistent.

### `/knvs:finalize` - Finalize Research Report
Incorporates a Maengelprotokoll (deficiency audit) into a draft research report. Applies strict editing protocol (delete hallucinations, precise context gaps, keep verified claims) and sets `status: verified`.

**When to use:** After an external audit of a draft research report, before `/knvs:impact`.

---

## File Structure

```
knvs/
|- .claude-plugin/               # Plugin manifest
|   |- plugin.json
|
|- skills/                       # Skill documentation (incl. templates)
|   |- start/SKILL.md
|   |- impact/SKILL.md
|   |- ideate/SKILL.md
|   |- ideas/SKILL.md
|   |- explore/SKILL.md
|   |- exploit/SKILL.md
|   |- review/SKILL.md
|   |- sync/SKILL.md
|
|- research/                     # Pre-ideate research reports
|   |- marktanalyse-verified.md
|
|- impacts/                      # Atomic impact analysis
|   |- marktanalyse--impact-slug.md
|
|- network/                      # Vendor/partner model snapshots (indirect impacts)
|   |- vendor-corp.md
|
|- ideate/                       # Ideas in research phase
|   |- ai-bookkeeping.md
|
|- explore/                      # Ideas being validated
|   |- fitness-seniors.md
|
|- exploit/                      # Validated & scaling
|   |- invoice-tool.md
|
|- reviews/                      # Disruption review history
|   |- ai-bookkeeping/
|       |- 2026-03-15.md
|
|- README.md                     # This file
|- QUICKSTART.md                 # Quick-start guide
```

**Phase Folders:**
- `research/` - External research reports, verified or draft (pre-ideate)
- `impacts/` - Atomic impact analysis from verified research
- `network/` - Vendor/partner model snapshots for indirect impacts
- `ideate/` - Ideas you're still researching and brainstorming
- `explore/` - Ideas with hypotheses being validated
- `exploit/` - Validated business models being scaled

**Your Canvas Files:**
- Are automatically created in `ideate/` via `/knvs:ideate`
- Move to `explore/` when you run `/knvs:explore`
- Move to `exploit/` when validation is complete
- The folder shows the current phase at a glance

---

## Documentation

### Skill Documentation
Each skill has detailed documentation in `skills/*/SKILL.md`:
- Workflows and examples
- Template structures
- Canvas lifecycle documentation

### Canvas Templates
Each skill contains the canonical template for its phase. See the `## Canvas Template` or `## Research Report Template` section in:
- [CLAUDE.md](CLAUDE.md) - Research report template (under "Research Report: Struktur")
- [skills/impact/SKILL.md](skills/impact/SKILL.md) - Impact atom template
- [skills/ideate/SKILL.md](skills/ideate/SKILL.md) - IDEATE canvas template
- [skills/explore/SKILL.md](skills/explore/SKILL.md) - EXPLORE canvas template
- [skills/exploit/SKILL.md](skills/exploit/SKILL.md) - EXPLOIT canvas template
- [skills/review/SKILL.md](skills/review/SKILL.md) - Disruption review template

---

## Workflow

### You do the content work
- Develop business ideas
- Fill in Business Model Canvas
- Define hypotheses
- Run experiments
- Collect and analyze data

### Claude Code supports you
- Structuring and formatting
- Identifying critical assumptions
- Suggestions for experiment designs
- Reminders for quality criteria
- Consistent documentation

---

## Markdown Format

All content in knvs is written in Markdown:

**Benefits:**
- Readable for humans (in Obsidian, Notion, VS Code)
- Machine-readable (Claude can understand and process content)
- Versionable (Git-friendly)
- Platform-independent (no lock-in)

**Recommended Editors:**
- Obsidian (excellent for linked notes)
- Notion (collaborative, cloud-based)
- VS Code (for tech-savvy users)

---

## Idea Lifecycle

### RESEARCH -> IMPACT -> IDEATE -> EXPLORE -> EXPLOIT

**Status in YAML frontmatter:**

Each canvas has metadata at the top:
```yaml
---
status: IDEATE
progress: WIP
created: 2026-01-24
---
```

**Progress States (IDEATE only):**
- **WIP** - Work in progress, still researching/brainstorming
- **READY FOR EXPLORE** - Research complete, ready to validate hypotheses

When you run `/knvs:explore`, the file moves to `explore/` folder and status changes:
```yaml
---
status: EXPLORE
created: 2026-01-24
updated: 2026-02-15
---
```

---

## Resources

### Strategyzer Methodology
knvs is based on the proven "Testing Business Ideas" methodology from Strategyzer:
- Systematic hypothesis validation
- Experiment-driven development
- Data-based decisions

### Community & Support
- Questions? Use Claude Code as a sparring partner
- See skill documentation in `skills/*/SKILL.md` for best practices

---

**Good luck developing and validating your business ideas!**
