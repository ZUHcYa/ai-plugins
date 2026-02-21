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

1. Copy the knvs template files to your project
2. Create the folder structure manually (see below)

### Updates

To get the latest version, pull the latest changes from the repository or re-download the template files.

---

## 1. Setup

```
/knvs:start
```

On first run, knvs automatically sets up your workspace:

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

See [STRUCTURE.md](STRUCTURE.md) for complete folder documentation.

---

## 2a. (Optional) Finalize Draft Research

If your research was AI-generated and externally audited:

```
/knvs:finalize
```

Claude reads the draft and audit file, applies corrections (deletes hallucinations, precises context gaps), and sets the report to `verified`.

---

## 2b. (Optional) Extract Impact Atoms

After verifying research, extract atomic impacts:

```
/knvs:impact
```

Claude reads the verified research, proposes individual impacts, and lets you confirm each one.
Impact atoms help structure how research findings pressure specific BMC areas.

---

## 3. Capture Your First Idea

```
/knvs:ideate
```

Describe your idea. Claude creates a Business Model Canvas with:
- All BMC sections (Value Proposition, Customer Segments, etc.)
- Status: IDEATE
- Progress: WIP

**Then:** Open the file and fill out all sections.

---

## 4. Validate Your Idea

When your canvas is complete, set `progress: READY FOR EXPLORE` and run:

```
/knvs:explore
```

Claude generates hypotheses from your canvas with test suggestions:
- Pricing hypotheses
- Customer segment assumptions
- Value proposition validation

**Then:** Run the experiments and document results (VALIDATED / INVALIDATED).

---

## 5. Scale Your Business Model

When all critical hypotheses are validated:

```
/knvs:exploit
```

Claude moves your canvas to the EXPLOIT phase and initializes:
- Review schedule
- Disruption risk monitoring

---

## The Complete Lifecycle

```
/knvs:start → /knvs:impact → /knvs:ideate → fill canvas → /knvs:explore → run tests → /knvs:exploit
     |              |              |              |              |              |             |
   Setup         Extract          Create       Research       Validate       Document       Scale
               (optional)     (optional)
```

---

## Check Your Status

Run `/knvs:start` anytime to see your portfolio status and suggested next actions:

```
User: /knvs:start
Claude: knvs Status
        ======================================

        IDEATE (3)
        ----------
        🔴 AI Bookkeeping [WIP] - 45 days stale
        🟢 Invoice Tool [READY] - run /knvs:explore

        Suggested Actions
        -----------------
        1. Invoice Tool is READY → /knvs:explore
```

---

## Helpful Skills

| Skill | When to use |
|-------|-------------|
| `/knvs:start` | Setup (first run) or overview + portfolio (after setup) |
| `/knvs:impact` | Extract atomic impacts from verified research |
| `/knvs:ideas` | List all open ideas |
| `/knvs:review` | Disruption check for EXPLOIT |
| `/knvs:sync` | Check for changes |

---

## Manual Workflow (No Skills)

knvs works without Claude Code skills too:

1. Create folders: `research/`, `impacts/`, `ideate/`, `explore/`, `exploit/`, `reviews/`
2. Create file: `ideate/my-idea.md`
3. Add frontmatter: `status: IDEATE`, `progress: WIP`
4. Fill out BMC sections
5. Move file: `ideate/` to `explore/`
6. Update status: `status: EXPLORE`

All files are standard Markdown - use any editor (Obsidian, VS Code, Notion, etc.).
