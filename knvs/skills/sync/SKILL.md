---
name: "sync"
description: "Check for file changes and consistency (file system only, no Git)"
---

# /knvs:sync

## Purpose

Reviews canvas and data file changes and flags inconsistencies on demand.

## When to Use

- After editing files in Obsidian or another editor
- To ensure everything is consistent
- Before important reviews or presentations
- Checking for stale or forgotten ideas

## Workflow

```
User: /knvs:sync
Claude: Scanning for changes...

        ======================================
        SYNC REPORT
        ======================================

        Modified (3):
          - explore/ai-bookkeeping.md - updated 2h ago
          - explore/fitness-app.md - updated yesterday
          - exploit/core-business.md - updated 3d ago

        New (1):
          - explore/new-idea.md - created today

        Removed (0):
          none

        Issues (2):
          - explore/old-idea.md has status: draft and is 30+ days old
            -> Consider: set status: testing or archive
          - explore/moved-idea.md has status: scaling (should be in exploit/)
            -> Should I move to exploit/?

        Sync complete!
```

## What the Skill Does

1. Scans `explore/`, `exploit/`, `hypotheses/`, `experiments/`, `insights/`, `archive/` folders
2. Detects modified/new/deleted files using file timestamps
3. Reads frontmatter from each file
4. Runs consistency checks
5. Reports findings grouped by type
6. Offers to fix detected issues

## Consistency Checks

| Check | Description | Warning |
|-------|-------------|---------|
| Status <-> Folder | Canvas in `explore/` has `status: scaling`? | Status/Folder Mismatch |
| Missing BMC Fields | Canvas lacks one of the 9 core BMC `##` headings? | Incomplete canvas |
| Missing Fields | Testing canvas without `innovation_risk`? | Missing field |
| Stale Draft | Draft older than 30 days? | Stale idea |
| Overdue Review | EXPLOIT `next_review` in past? | Review overdue |
| Invalid Canvas | .md file without valid frontmatter? | Invalid canvas |
| Orphaned Hypothesis | Hypothesis `canvas` points to non-existent canvas file? | Broken reference |
| Hypothesis Missing Claim | Hypothesis without required sections (Claim, Context)? | Incomplete hypothesis |
| Hypothesis Testing No Experiment | Hypothesis `status: testing` but no experiment in `experiments/`? | No running experiment |
| Experiment No Hypothesis | Experiment `hypothesis` points to non-existent file? | Broken reference |
| Experiment Stale | Experiment `status: running` older than 30 days? | Stale experiment |
| Completed Experiment No Insights | Experiment `status: completed` with no insights in `insights/`? | Missing insights — run /knvs:learn |
| Insight No Experiment | Insight `source_experiment` points to non-existent file? | Broken reference |
| Orphaned Data Folder | `hypotheses/<slug>/` or `experiments/<slug>/` without matching canvas? | Orphaned data — archive or delete |

## Notes

- Sync is on-demand, not automatic
- User has full control over when to check for changes
- Claude offers to fix detected issues but waits for confirmation
