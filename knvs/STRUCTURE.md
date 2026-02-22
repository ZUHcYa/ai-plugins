# knvs Folder Structure

Overview of the knvs folder organization.

---

## Phase Folders (Your Content)

These folders contain your business model canvases.

| Folder | Purpose | Status Values |
|--------|---------|---------------|
| `explore/` | BMCs being developed and validated | `draft`, `testing`, `validated` |
| `exploit/` | Validated business models being scaled | `scaling` |
| `archive/` | Killed or pivoted canvases (reference) | — |

**Lifecycle:** `/knvs:ideate` → `explore/` (draft) → set `status: testing` → validate → `/knvs:exploit` → `exploit/`

**Pivot:** `explore/` → `archive/` (old) + `explore/` (new variant as draft)

**Kill:** `explore/` → `archive/`

---

## Data Folders

These folders contain hypotheses, experiments, and insights grouped by canvas.

| Folder | Purpose | Created by |
|--------|---------|------------|
| `hypotheses/<canvas-slug>/` | Testable assumptions from BMC | `/knvs:hypothesize` |
| `experiments/<canvas-slug>/` | Experiment designs and results | `/knvs:experiment` |
| `insights/<canvas-slug>/` | Key learnings from experiments | `/knvs:learn` |

Each canvas in `explore/` (with `status: testing`) has corresponding subfolders in these directories.

---

## Review Folders

| Folder | Purpose |
|--------|---------|
| `reviews/` | Disruption review history |
| `reviews/<canvas>/` | Reviews for a specific canvas |

Reviews are stored separately from canvases for better history tracking.

**Example:**
```
reviews/
└── ai-bookkeeping/
    ├── 2026-03-15.md
    └── 2026-06-15.md
```

---

## Reference Folders (Read Only)

| Folder | Purpose |
|--------|---------|
| `skills/` | Skill documentation (including templates) |

---

## Configuration

| Location | Purpose |
|----------|---------|
| `.knvs/config.json` | knvs settings (targetPath) |

---

## Complete Structure

```
<targetPath>/
├── .knvs/
│   └── config.json              # Configuration
├── explore/                     # Your BMCs (draft + testing)
├── exploit/                     # Your scaling business models
├── archive/                     # Killed/pivoted canvases
├── hypotheses/                  # Hypotheses per canvas
│   └── <canvas-slug>/
├── experiments/                 # Experiments per canvas
│   └── <canvas-slug>/
├── insights/                    # Insights per canvas
│   └── <canvas-slug>/
├── reviews/                     # Disruption review history
│   └── <canvas-slug>/
├── skills/
│   ├── start/SKILL.md
│   ├── ideate/SKILL.md
│   ├── hypothesize/SKILL.md
│   ├── experiment/SKILL.md
│   ├── learn/SKILL.md
│   ├── decide/SKILL.md
│   ├── exploit/SKILL.md
│   ├── review/SKILL.md
│   └── sync/SKILL.md
├── CLAUDE.md
├── README.md
├── QUICKSTART.md
└── STRUCTURE.md
```

---

## File Naming

| Type | Convention | Example |
|------|------------|---------|
| Canvas files | kebab-case slug | `ai-bookkeeping.md` |
| Hypothesis files | kebab-case slug | `customers-will-pay-monthly.md` |
| Experiment files | kebab-case slug | `pricing-survey-freelancers.md` |
| Insight files | kebab-case slug | `freelancers-prefer-annual.md` |
| Skills | `<skill>/SKILL.md` | `hypothesize/SKILL.md` |
| Config | Standard name | `config.json` |

---

**Version:** 2.0.0
