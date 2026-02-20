# knvs Folder Structure

Overview of the knvs folder organization.

---

## Pre-Phase Folders

| Folder | Purpose | Status |
|--------|---------|--------|
| `research/` | Research reports (verified or draft) and hypothesis briefs (pre-ideate) | RESEARCH |
| `impacts/` | Atomic impact analysis from verified research | IMPACT |
| `network/` | Vendor, partner, customer and competitor model snapshots | NETWORK |

Research reports are external analyses (verified or trusted) that may later inform business ideas.
Hypotheses (`status: hypothesis`) are unverified market assumptions captured as research briefs.
Both are NOT part of the innovation lifecycle until transformed via `/knvs:ideate`.

Impact Atoms are extracted from research reports via `/knvs:impact`.
Each atom describes one specific pressure or opportunity for business models.
They are referenced during BMC creation (`/knvs:ideate`) and disruption reviews (`/knvs:review`).

Network Canvases (`type: network-canvas`) capture the business models of external parties.
They are the home for **indirect impacts** - impacts that primarily affect a vendor or partner's
model, and only secondarily cascade to our own canvas. Our canvas shows a lightweight
`[!vendor-risk]` reference callout pointing to the network canvas, not the full impact content.
Create on-demand when a vendor or partner becomes strategically relevant.

---

## Phase Folders (Your Content)

These folders contain your business model canvases. Files move between phases as ideas progress.

| Folder | Purpose | Status |
|--------|---------|--------|
| `ideate/` | New ideas being researched | IDEATE |
| `explore/` | Ideas being validated with experiments | EXPLORE |
| `exploit/` | Validated business models being scaled | EXPLOIT |

**Lifecycle (direct impacts):** `research/` → `impacts/` → `/knvs:ideate` → `ideate/` → `explore/` → `exploit/`

**Lifecycle (indirect impacts via vendor):** `research/` → `/knvs:impact` → `network/<vendor>.md` → `[!vendor-risk]` in own canvas

---

## Review Folders

| Folder | Purpose |
|--------|---------|
| `reviews/` | Disruption review history |
| `reviews/<canvas>/` | Reviews for a specific canvas |

Reviews are stored separately from canvases for better history tracking. Each canvas in `exploit/` has a corresponding folder in `reviews/`.

**Example:**
```
reviews/
└── ai-bookkeeping/
    ├── 2026-03-15.md
    └── 2026-06-15.md
```

---

## Reference Folders (Read Only)

These folders contain skill documentation. Do not modify.

| Folder | Purpose |
|--------|---------|
| `skills/` | Skill documentation (including canvas templates) |

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
│   └── config.json          # Configuration
├── research/                 # Pre-ideate research reports
├── impacts/                  # Atomic impact analysis (direct impacts on our models)
├── network/                  # Vendor/partner model snapshots (indirect impacts)
│   └── vendor-corp.md       # Example: type: network-canvas
├── ideate/                   # Your IDEATE canvases
├── explore/                  # Your EXPLORE canvases
├── exploit/                  # Your EXPLOIT canvases
├── reviews/                  # Disruption review history
│   └── <canvas-name>/       # Reviews per canvas
├── skills/
│   ├── start/SKILL.md       # Smart entry point
│   ├── impact/SKILL.md      # Extract impact atoms
│   ├── ideate/SKILL.md      # Create new idea
│   ├── ideas/SKILL.md       # List open ideas
│   ├── explore/SKILL.md     # Validate hypotheses
│   ├── exploit/SKILL.md     # Scale business model
│   ├── review/SKILL.md      # Disruption review
│   └── sync/SKILL.md        # Check for changes
├── CLAUDE.md                 # Claude instructions
├── README.md                 # Process documentation
├── QUICKSTART.md             # Getting started guide
└── STRUCTURE.md              # This file
```

---

## File Naming

| Type | Convention | Example |
|------|------------|---------|
| Canvas files | Descriptive, kebab-case | `ai-bookkeeping.md` |
| Skills | `<skill>/SKILL.md` | `start/SKILL.md` |
| Config | Standard name | `config.json` |

---

## Target Path

By default, knvs creates files in the current directory (`./`).

During `/knvs:start`, you can specify a custom target folder. All knvs files (phase folders, config) will be created there instead.

**Example:** If `targetPath: "./innovation"`, the structure becomes:

```
./innovation/
├── .knvs/config.json
├── ideate/
├── explore/
├── exploit/
└── ...
```

---

**Version:** 0.13.0
