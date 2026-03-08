# research Quick-Start Guide

Get started with research in 5 minutes.

---

## 1. Setup

```
/research:start
```

On first run, research asks for your preferences:

```
User: /research:start
Claude: Welcome to research!

        Where should research reports be stored? [Default: ./research]
User: ./research

Claude: Created: .research/config.json
        Created: research/

        Ready! Create a draft report or import one from an external AI session.
```

---

## 2. The Lifecycle

### Step 1: Investigate (NEW)

Research a topic with structured web search and source evaluation:

```
/research:investigate machine customers B2B
```

Claude searches the web, evaluates sources with the CRAAP framework,
and produces a draft report with cited evidence and quality grades.

### Step 2: Evaluate (NEW — optional)

Self-critique the draft before external audit:

```
/research:evaluate research/machine-customers-b2b.md
```

Detects logical fallacies, cognitive biases, unbacked claims.
Fix flagged issues before proceeding.

### Step 3: Audit

Run an external audit in a separate AI session (e.g. Claude Opus with Extended Thinking).
The audit prompt is in `CLAUDE.md`. Save the result as:

```
research/machine-customers-b2b-audit.md
```

### Step 4: Finalize

```
/research:finalize
```

Claude applies the audit findings (RED: delete, AMBER: precise, GREEN: keep)
and sets `status: verified`.

### Alternative: Systematic Review (NEW)

For focused, answerable questions, use `/research:review` instead:

```
/research:review "Does AI-driven pricing increase B2B churn?"
```

Searches multiple sources, compares findings, documents contradictions.

---

## 3. Check Your Status

```
/research:start
```

Run anytime to see all reports with their status:

```
User: /research:start
Claude: Research Pipeline Overview
        ======================================

        Reports (3)
        ----------------------
        machine-customers.md     verified  2026-02-22
        saas-pricing.md          draft     2026-02-20
        competitor-analysis.md   draft     2026-02-19

        Audit-Ready (1)
        ----------------
        saas-pricing.md has audit file -> /research:finalize

        Actions
        -------
        /research:finalize to incorporate audit findings
```

---

## The Complete Workflow

```
/research:start -> /research:investigate -> /research:evaluate -> Audit -> /research:finalize -> /research:start
     |                    |                       |                  |              |                     |
   Setup           Web research            Self-critique      External      Incorporate audit     Review status
                   (draft report)          (optional)         audit file     (RED/AMBER/GREEN)
```

For focused questions:
```
/research:review "Question?" -> Standalone review document (no audit cycle)
```

---

## Skills

| Skill | When to use |
|-------|-------------|
| `/research:start` | Setup (first run) or overview (after setup) |
| `/research:investigate` | Research a topic with web search and source evaluation |
| `/research:evaluate` | Self-critique a draft for critical thinking quality |
| `/research:finalize` | Incorporate audit findings into a draft report |
| `/research:review` | Systematic review of a focused research question |

---

## Manual Workflow (No Skills)

research works without Claude Code skills too:

1. Create folder: `research/`
2. Create draft: `research/my-topic.md` with `status: draft`
3. Get external audit: save as `research/my-topic-audit.md`
4. Apply RED/AMBER/GREEN edits manually
5. Set `status: verified` and add `verified: YYYY-MM-DD`

All files are standard Markdown — use any editor (Obsidian, VS Code, Notion, etc.).
