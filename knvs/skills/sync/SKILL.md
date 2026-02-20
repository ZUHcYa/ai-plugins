---
name: "sync"
description: "Check for file changes (file system only, no Git)"
---

# /knvs:sync

## Purpose

Reviews canvas changes and flags inconsistencies on demand.

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
          - ideate/fitness-app.md - updated yesterday
          - exploit/core-business.md - updated 3d ago

        New (1):
          - ideate/new-idea.md - created today

        Removed (0):
          none

        Issues (2):
          - ideate/old-idea.md has progress: READY but is 30+ days old
            -> Consider: run /knvs:explore or archive
          - explore/moved-idea.md has status: IDEATE (folder mismatch)
            -> Should I update to EXPLORE?

        Sync complete!
```

## What the Skill Does

1. Scans `research/`, `impacts/`, `network/`, `ideate/`, `explore/`, `exploit/` folders
2. Detects modified/new/deleted files using file timestamps
3. Reads frontmatter from each canvas
4. Runs consistency checks
5. Reports findings grouped by type
6. Offers to fix detected issues

## Consistency Checks

| Check | Description | Warning |
|-------|-------------|---------|
| Stale Research | RESEARCH `status: draft` older than 14 days? | Stale research |
| Orphaned Research | RESEARCH `status: verified` without matching IDEATE canvas >30 days? | Unused research |
| Orphaned Impact | Impact atom with `driver` pointing to non-existent research file? | Orphaned impact |
| Impact Missing Fields | Impact atom without `bmc_fields` array? | Missing field |
| Stale Driver | Impact `driver` links to research with `status: draft`? | Driver unverified (advisory) |
| Duplicate Impact | Two atoms with identical `title` and overlapping `bmc_fields`? | Potential duplicate |
| Invalid Impact | File in `impacts/` without `type: impact-atom`? | Invalid impact |
| Folder <-> Status | Canvas in `ideate/` has `status: EXPLORE`? | Status/Folder Mismatch |
| Missing Fields | EXPLORE without `innovation_risk`? | Missing field |
| Stale WIP | IDEATE WIP older than 30 days? | Stale idea |
| Overdue Review | EXPLOIT `next_review` in past? | Review overdue |
| Missing BMC Fields | Canvas lacks one of the 9 core BMC `##` headings? | Incomplete canvas |
| Invalid Canvas | .md file without valid frontmatter? | Invalid canvas |
| Deprecated Impact Section | Canvas has `## Impact Context` section instead of inline callouts? | Deprecated format - consider migrating |
| Orphaned Inline Impact | Inline impact `_Quelle: [[impacts/slug]]_` references non-existent file? | Broken impact reference |
| Stale Hypothesis | RESEARCH `status: hypothesis` older than 14 days? | Stale hypothesis - research or delete |
| Orphaned Hypothesis | Hypothesis `origin_impact` points to non-existent file? | Broken reference - remove field or delete |
| Resolved Hypothesis | Hypothesis slug has matching `<slug>-verified.md`? | Hypothesis possibly resolved - review and delete |
| Invalid Hypothesis | Hypothesis file has draft/verified-only fields (`source_report`, `deficiency_list`, `corrections_applied`)? | Invalid hypothesis format |
| Hypothesis Missing Claim | Hypothesis without `claim` field? | Missing required field |
| Stale Network Canvas | `network/*.md` with `snapshot_date` older than 90 days? | Stale vendor snapshot - review or update |
| Missing Snapshot Date | `type: network-canvas` without `snapshot_date` field? | Missing required field |
| Broken Vendor Risk Ref | `[!vendor-risk]` callout links to non-existent `network/` file? | Broken vendor reference - create or remove |
| Invalid Network Canvas | .md file in `network/` without `type: network-canvas`? | Invalid network canvas format |

## Notes

- Sync is on-demand, not automatic
- User has full control over when to check for changes
- Claude offers to fix detected issues but waits for confirmation

