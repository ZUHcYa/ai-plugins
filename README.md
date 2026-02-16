# ai-plugins

**AI Plugins** - Curated Claude Code plugins for diverse workflows.

A marketplace of specialized plugins covering various domains:
- **Creative workflows** (Music Production)
- **Productivity & alignment** (Session Goal Tracking)
- _...and more to come_

Each plugin follows a tool-agnostic philosophy: all workflows work with plain Markdown in any editor (Obsidian, VS Code, Notion). Claude Code skills are optional helpers, never requirements.

This is a monorepo containing the plugin marketplace and all plugin source code.

---

## Installation

### Claude Code

Install any plugin from this hub:

```bash
/plugin install <plugin-name>@github.com/ZUHcYa/ai-hub
```

### Example

```bash
/plugin install knvs@github.com/ZUHcYa/ai-hub
```

---

## Available Plugins

| Plugin | Description | Status |
|--------|-------------|--------|
| [**compass**](compass/) | Session Goal Alignment - Vision tracking and drift detection | Stable |
| [**music-production**](music-production/) | Music Production Workflow Companion | Scaffold |

See each plugin's README for details:
- [compass/README.md](compass/README.md)

---

## Repository Structure

```
ai-hub/
├── .claude-plugin/
│   └── marketplace.json      # Plugin registry (marketplace)
├── compass/                  # Session Goal Alignment
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── CLAUDE.md
│   ├── skills/
│   └── hooks/
├── music-production/         # Music Production (scaffold)
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── CLAUDE.md
│   └── skills/
└── README.md
```

---

## Adding Plugins

To add a new plugin to this hub:

1. Create a new folder in the repository root (short name, e.g. `myplugin/`)
2. Add a `.claude-plugin/plugin.json` manifest
3. Add a `CLAUDE.md` with plugin-specific dev rules
4. Add `skills/` and optionally `hooks/` as needed
5. Register the plugin in `.claude-plugin/marketplace.json`

---

## License

MIT
