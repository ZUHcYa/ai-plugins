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

### Step 1: Draft

Create (or import) a research report with `status: draft`:

```
research/machine-customers.md
---
type: research
title: "Machine Customers"
status: draft
created: 2026-02-22
---

# Research Report: Machine Customers

## Summary
...
```

### Step 2: Audit

Run an external audit in a separate AI session (e.g. Claude Opus with Extended Thinking).
The audit prompt is in `CLAUDE.md`. Save the result as:

```
research/machine-customers-audit.md
```

### Step 3: Finalize

```
/research:finalize
```

Claude applies the audit findings (RED: delete, AMBER: precise, GREEN: keep)
and sets `status: verified`.

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
/research:start -> Draft -> Audit -> /research:finalize -> /research:start
     |                                      |                     |
   Setup                           Incorporate audit        Review status
```

---

## Skills

| Skill | When to use |
|-------|-------------|
| `/research:start` | Setup (first run) or overview (after setup) |
| `/research:finalize` | Incorporate audit findings into a draft report |

---

## Manual Workflow (No Skills)

research works without Claude Code skills too:

1. Create folder: `research/`
2. Create draft: `research/my-topic.md` with `status: draft`
3. Get external audit: save as `research/my-topic-audit.md`
4. Apply RED/AMBER/GREEN edits manually
5. Set `status: verified` and add `verified: YYYY-MM-DD`

All files are standard Markdown — use any editor (Obsidian, VS Code, Notion, etc.).
