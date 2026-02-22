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
Draft → Audit → Finalize → Verified

1. Create or import a draft research report (status: draft)
2. Run an external audit (Maengelprotokoll) in a separate AI session
3. /research:finalize incorporates audit findings
4. Result: verified research report ready for use
```

---

## Available Skills

### `/research:start` - Smart Entry Point
Setup (first run) or overview (after setup). Shows all reports with status and suggested actions.

### `/research:finalize` - Finalize Research Report
Incorporates a Maengelprotokoll into a draft research report. Applies strict editing protocol
(RED: delete, AMBER: precise, GREEN: keep) and sets `status: verified`.

---

## File Structure

```
research/
|- .claude-plugin/
|   |- plugin.json
|
|- skills/
|   |- start/SKILL.md
|   |- finalize/SKILL.md
|
|- research/                    # Your research reports
|   |- topic-analysis.md        # status: draft or verified
|   |- topic-analysis-audit.md  # External audit file
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
