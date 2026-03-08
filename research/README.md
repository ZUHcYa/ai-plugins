# research - Structured Research Pipeline

**Draft, audit, and verify research reports with a structured workflow.**

---

## What is this?

**research** is a Claude Code plugin for managing research reports through a structured lifecycle:
draft, audit, and verification. It ensures research quality through an external audit process.

### Who is this for?

Anyone who needs structured, verified research reports — whether for business model validation,
market analysis, technology assessment, or strategic decision-making.

---

## Installation

### Claude Code Plugin

**Step 1:** Add the marketplace
```
/plugin marketplace add ZUHcYa/ai-plugins
```

**Step 2:** Install the plugin
```
/plugin install research@ai-plugins
```

**Step 3:** Run the setup
```
/research:start
```

---

## The Research Lifecycle

```
Investigate → Evaluate → Audit → Finalize → Verified

1. /research:investigate researches a topic with source evaluation (NEW)
2. /research:evaluate self-critiques the draft for critical thinking quality (NEW)
3. Run an external audit (Maengelprotokoll) in a separate AI session
4. /research:finalize incorporates audit findings
5. Result: verified research report ready for use

For focused questions, use /research:review for a systematic review instead.
```

---

## Available Skills

### `/research:start` - Smart Entry Point
Setup (first run) or overview (after setup). Shows all reports with status and suggested actions.

### `/research:investigate` - Structured Web Research
Conducts structured research on a topic using WebSearch. Evaluates every source with the
CRAAP framework, maps claims to evidence, flags evidence gaps, and produces a draft report.

### `/research:evaluate` - Critical Thinking Analysis
Analyzes a draft for logical fallacies, cognitive biases, unbacked claims, and argument
structure weaknesses (Toulmin model). Self-critique before external audit.

### `/research:finalize` - Finalize Research Report
Incorporates a Maengelprotokoll into a draft research report. Applies strict editing protocol
(RED: delete, AMBER: precise, GREEN: keep) and sets `status: verified`.

### `/research:review` - Systematic Review Lite
Answers a focused research question by searching multiple sources, comparing findings,
documenting contradictions and consensus. Uses PICO decomposition and PRISMA-lite screening.

---

## File Structure

```
research/
|- .claude-plugin/
|   |- plugin.json
|
|- skills/
|   |- start/SKILL.md
|   |- investigate/SKILL.md
|   |- evaluate/SKILL.md
|   |- finalize/SKILL.md
|   |- review/SKILL.md
|
|- research/                           # Your research reports
|   |- topic-analysis.md               # status: draft or verified
|   |- topic-analysis-audit.md         # External audit file
|   |- topic-analysis-evaluation.md    # Self-critique output
|   |- focused-question-review.md      # Systematic review output
|
|- CLAUDE.md
|- README.md
|- CHANGELOG.md
```

---

## Manual Workflow (No Skills)

This plugin works without Claude Code:

1. Create folder: `research/`
2. Create file: `research/my-topic.md` with `status: draft`
3. Get an external audit → save as `research/my-topic-audit.md`
4. Apply RED/AMBER/GREEN edits manually
5. Set `status: verified` and add `verified: YYYY-MM-DD`

All files are standard Markdown — use any editor.

---

## Works well with

### vntrs plugin

The [vntrs](https://git.friedners.eu/friedrich/vntrs) plugin uses verified research reports
as input for hypothesis generation. No configuration needed — vntrs reads the standard report
format directly.

The plugins are independent — research works fully without vntrs, and vntrs works fully without research.
