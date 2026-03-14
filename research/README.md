# research - Structured Research Pipeline

**Draft, audit, and verify research reports with a structured workflow.**

## What is this?

**research** manages research reports through a structured lifecycle:
draft, audit, and verification. It ensures research quality through an external audit process.

## Quick Start

```
/research:start                              Setup on first run, overview afterwards
/research:investigate machine customers B2B  Research a topic with web search
/research:evaluate research/topic.md         Self-critique a draft (optional)
/research:finalize                           Incorporate audit findings
/research:review "Does X cause Y?"           Systematic review of a focused question
```

## The Research Lifecycle

```
Investigate → Evaluate → Audit → Finalize → Verified

1. /research:investigate  researches a topic with source evaluation
2. /research:evaluate     self-critiques the draft for critical thinking quality
3. Run an external audit (Maengelprotokoll) in a separate AI session
4. /research:finalize     incorporates audit findings
5. Result: verified research report ready for use

For focused questions, use /research:review for a systematic review instead.
```

## Skills

| Skill | Purpose |
|-------|---------|
| `/research:start` | Setup (first run) or overview (after setup) |
| `/research:investigate` | Structured web research with CRAAP source evaluation |
| `/research:evaluate` | Critical thinking analysis (fallacies, biases, unbacked claims) |
| `/research:finalize` | Incorporate audit findings (RED: delete, AMBER: precise, GREEN: keep) |
| `/research:review` | Systematic review of a focused research question (PICO + PRISMA-lite) |

## File Structure

```
research/
├── .research/config.json              Config (dataDir)
├── skills/                            Skill definitions
├── research/                          Your research reports
│   ├── topic-analysis.md              status: draft or verified
│   ├── topic-analysis-audit.md        External audit file
│   ├── topic-analysis-evaluation.md   Self-critique output
│   └── focused-question-review.md     Systematic review output
├── CLAUDE.md
├── README.md
├── STRUCTURE.md
└── CHANGELOG.md
```

## Manual Workflow (No Skills)

This plugin works without Claude Code:

1. Create folder: `research/`
2. Create file: `research/my-topic.md` with `status: draft`
3. Get an external audit → save as `research/my-topic-audit.md`
4. Apply RED/AMBER/GREEN edits manually
5. Set `status: verified` and add `verified: YYYY-MM-DD`

All files are standard Markdown — use any editor.

## Works well with

### vntrs plugin

The [vntrs](https://git.friedners.eu/friedrich/vntrs) plugin uses verified research reports
as input for hypothesis generation. No configuration needed — vntrs reads the standard report
format directly. The plugins are independent.

## Documentation

- [STRUCTURE.md](STRUCTURE.md) - File formats and YAML schema reference
- [CHANGELOG.md](CHANGELOG.md) - Version history
