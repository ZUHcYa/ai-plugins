# knvs Folder Structure

Overview of the knvs folder organization.

---

## Phase Folders

These folders contain your business model canvases, each in its own self-contained subfolder.

| Folder | Purpose | Status Values |
|--------|---------|---------------|
| `explore/` | Canvases being developed and validated | `draft`, `testing`, `validated` |
| `exploit/` | Validated business models being scaled | `scaling` |
| `archive/` | Stopped or pivoted canvases (reference) | — |

**Lifecycle:** `/knvs:ideate` → `explore/<slug>/` (draft) → set `status: testing` → validate → `/knvs:exploit` → `exploit/<slug>/`

**Pivot:** `explore/<slug>/` → `archive/<slug>/` (old) + `explore/<new-slug>/` (new variant as draft)

**Stop:** `explore/<slug>/` → `archive/<slug>/` (one operation)

---

## Canvas Subfolders

Each canvas has its own folder containing all related data. Subfolders are created on-demand by the respective skills.

| Subfolder | Purpose | Created by |
|-----------|---------|------------|
| `hypotheses/` | Testable assumptions from canvas | `/knvs:hypothesize` |
| `experiments/` | Experiment designs and results | `/knvs:experiment`, `/knvs:capture` |
| `insights/` | Key learnings from experiments | `/knvs:learn` |
| `learning-cards/` | Learning Cards — Experiment + Hypothesis Cards | `/knvs:card` |
| `assessments/` | Performance & Trend assessments (EXPLOIT only) | `/knvs:assess` |

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
├── explore/                     # Your Canvases (draft + testing)
│   └── <canvas-slug>/
│       ├── <canvas-slug>.md     # Canvas file
│       ├── hypotheses/          # Hypotheses for this canvas
│       ├── experiments/         # Experiments for this canvas
│       ├── insights/            # Insights for this canvas
│       └── learning-cards/      # Learning Cards for this canvas
├── exploit/                     # Your scaling business models
│   └── <canvas-slug>/
│       ├── <canvas-slug>.md     # Canvas file
│       ├── hypotheses/          # Carried over from explore
│       ├── experiments/         # Carried over
│       ├── insights/            # Carried over
│       ├── learning-cards/      # Carried over
│       └── assessments/         # Performance & Trend assessments
├── archive/                     # Stoped/pivoted canvases
│   └── <canvas-slug>/          # Everything preserved as-is
├── skills/
│   ├── start/SKILL.md
│   ├── ideate/SKILL.md
│   ├── hypothesize/SKILL.md
│   ├── experiment/SKILL.md
│   ├── capture/SKILL.md
│   ├── learn/SKILL.md
│   ├── card/SKILL.md
│   ├── exploit/SKILL.md
│   └── assess/SKILL.md
├── CLAUDE.md
├── README.md
├── STRUCTURE.md
└── CHANGELOG.md
```

---

## File Naming

| Type | Convention | Example |
|------|------------|---------|
| Canvas folder | kebab-case slug | `ai-bookkeeping/` |
| Canvas file | slug inside folder | `ai-bookkeeping/ai-bookkeeping.md` |
| Hypothesis files | kebab-case slug | `customers-will-pay-monthly.md` |
| Experiment files | kebab-case slug | `pricing-survey-freelancers.md` |
| Insight files | kebab-case slug | `freelancers-prefer-annual.md` |
| Learning Card (experiment) | experiment-slug | `pricing-survey-freelancers.md` |
| Learning Card (hypothesis) | hypothesis-slug | `customers-will-pay-monthly.md` |
| Assessment files | date + type | `2026-03-04-performance.md` |
| Skills | `<skill>/SKILL.md` | `hypothesize/SKILL.md` |
| Config | Standard name | `config.json` |

---

**Version:** 7.0.0
