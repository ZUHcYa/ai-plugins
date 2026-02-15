# compass - Session Goal Alignment

A critical companion plugin for Claude Code that keeps your work focused on what matters.

## What It Does

**compass** prevents you from drifting off course during coding sessions by:

- **Defining goals** on two levels: persistent project vision + per-session objectives
- **Automatically checking** alignment before file creation and shell commands
- **Detecting drift** after each response and suggesting course corrections
- **Challenging assumptions** through devil's advocate deep-dives

## Quick Start

```
/compass:init          Set up vision tracking for your project
/compass:focus         Define your session goal
/compass:check         Manual alignment review
/compass:challenge     Devil's advocate mode
/compass:retro         Session retrospective
```

## How It Works

### Automatic (Hooks)

1. **Session Start** - Reminds you to set a session goal
2. **Before Actions** - Checks Write and Bash commands against your goals (warns, never blocks)
3. **After Responses** - Detects drift and suggests reviews when needed

### Manual (Skills)

| Skill | Purpose |
|-------|---------|
| `/compass:init` | One-time setup: creates `.compass/` with vision, rules, and session files |
| `/compass:focus` | Set or change your session goal |
| `/compass:check` | Deep alignment analysis with concrete recommendations |
| `/compass:challenge` | Actively question your approach and assumptions |
| `/compass:retro` | Session retrospective: what was achieved vs. planned |

### Built-in Heuristics

The automatic checks flag common engineering anti-patterns:

- **YAGNI** - Building something not needed for the current goal
- **SCOPE CREEP** - Adding features beyond the original request
- **OVER-ENGINEERING** - Unnecessary abstraction or premature generalization
- **GOLD PLATING** - Polishing beyond what's useful
- **PREMATURE OPT** - Optimizing without evidence of a bottleneck
- **NIH SYNDROME** - Rebuilding something that already exists

### Custom Rules

Edit `.compass/rules.md` to add project-specific constraints that the compass checks against.

## State Files

All state lives in `.compass/` in your project root:

```
.compass/
  VISION.md     Persistent project vision, anti-goals, constraints
  rules.md      Custom rules and anti-patterns
  session.md    Current session goal (refreshed per session)
```

Files are plain markdown with YAML frontmatter - readable in any editor, version-controllable with git.
