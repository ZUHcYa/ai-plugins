# compass Quick-Start Guide

Get started with compass in 5 minutes.

---

## Installation

### Option A: Claude Code Plugin (Recommended)

**Step 1:** Add the marketplace
```
/plugin marketplace add ZUHcYa/ai-plugins
```

**Step 2:** Install the plugin
```
/plugin install compass@ai-plugins
```

**Step 3:** Initialize vision tracking
```
/compass:init
```

### Option B: Manual Setup

1. Create a `.compass/` folder in your project root
2. Add `VISION.md`, `rules.md`, and `session.md` using the templates from `skills/init/SKILL.md`

### Updates

To get the latest version, pull the latest changes from the repository or re-install the plugin.

---

## 1. Initialize Vision Tracking

```
/compass:init
```

On first run, compass guides you through 3 questions:

```
User: /compass:init
Claude: No .compass/ directory found. Let's set up vision tracking.

        First question: What is the overarching goal of this project?

User: Build a marketplace for Claude Code plugins

Claude: Got it. Now: What should this project explicitly NOT do?
        (Anti-goals help prevent scope creep)

User: No paid features, no user accounts, no hosting infrastructure

Claude: Last question: What constraints apply?
        (Time, tech stack, scope limits)

User: Pure markdown plugins, no compiled code, ship MVP in 2 weeks

Claude: Created .compass/ structure:
          VISION.md     (project vision with goals, anti-goals, constraints)
          rules.md      (default rules + custom slot)
          session.md    (empty - will be set at session start)

        Vision tracking active!
        The SessionStart hook will show your vision at the start of each session.
        Use /compass:focus to set a session goal. Use /compass:check anytime for a manual alignment review.
```

---

## 2. Set a Session Goal

```
/compass:focus
```

At the start of each work session, tell compass what you're working on today. This gives the hooks context to evaluate alignment.

---

## 3. Hooks Run Automatically

Once compass is initialized, three hooks run in the background:

| Hook | When | What it does |
|------|------|--------------|
| **SessionStart** | Every session | Shows current vision + session goal |
| **PreToolUse (Write/Bash)** | Before file writes and shell commands | Checks alignment, warns if off-track |
| **Stop** | After each response | Detects drift from session goal |

All hooks are **advisory only** — they warn, but never block your work.

---

## 4. Review Alignment

```
/compass:check
```

Manual alignment review: compares recent changes against your vision and session goal, checks for scope creep.

---

## 5. Challenge Your Approach

```
/compass:challenge
```

Strategic questioning: compass plays devil's advocate and challenges your current approach. Useful when you feel stuck or want to stress-test a decision.

---

## 6. Wrap Up a Session

```
/compass:retro
```

Session retrospective: reviews what was accomplished, documents learnings, and resets the session goal.

---

## The Complete Workflow

```
/compass:init → /compass:focus → work → /compass:check → /compass:challenge → /compass:retro
     |               |            |           |                 |                    |
  Setup          Set goal     Auto hooks   Manual review   Stress-test          Wrap up
```

---

## Helpful Skills

| Skill | When to use |
|-------|-------------|
| `/compass:init` | First time, or to rebuild vision from scratch |
| `/compass:focus` | Start of each work session |
| `/compass:check` | Anytime you want a manual alignment review |
| `/compass:challenge` | When you want to stress-test your approach |
| `/compass:retro` | End of session |

---

## Manual Workflow (No Skills)

compass works without Claude Code skills too:

1. Create `.compass/VISION.md` with your goal, anti-goals, and constraints
2. Create `.compass/rules.md` with project-specific rules
3. Before each session: update `.compass/session.md` with today's goal
4. Periodically compare your work against your vision manually

All files are standard Markdown — use any editor (Obsidian, VS Code, Notion, etc.).
