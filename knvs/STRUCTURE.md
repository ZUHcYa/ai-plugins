# knvs Folder Structure

Overview of the knvs folder organization.

---

## Phase Folders (Your Content)

These folders contain your business model canvases. Files move between phases as ideas progress.

| Folder | Purpose | Status |
|--------|---------|--------|
| `ideate/` | New ideas being researched | IDEATE |
| `explore/` | Ideas being validated with experiments | EXPLORE |
| `exploit/` | Validated business models being scaled | EXPLOIT |
| `archive/` | Killed or pivoted canvases (reference) | ARCHIVED |

**Lifecycle:** `/knvs:ideate` → `ideate/` → `/knvs:explore` → `explore/` → `/knvs:exploit` → `exploit/`

**Pivot:** `explore/` → `archive/` (old) + `ideate/` (new variant)

**Kill:** `explore/` → `archive/`

---

## Data Folders

These folders contain hypotheses, experiments, and insights grouped by canvas.

| Folder | Purpose | Created by |
|--------|---------|------------|
| `hypotheses/<canvas-slug>/` | Testable assumptions from BMC | `/knvs:hypothesize` |
| `experiments/<canvas-slug>/` | Experiment designs and results | `/knvs:experiment` |
| `insights/<canvas-slug>/` | Key learnings from experiments | `/knvs:learn` |

Each canvas in `explore/` has corresponding subfolders in these directories.

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
├── ideate/                      # Your IDEATE canvases
├── explore/                     # Your EXPLORE canvases
├── exploit/                     # Your EXPLOIT canvases
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
│   ├── ideas/SKILL.md
│   ├── explore/SKILL.md
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

**Version:** 1.0.0
