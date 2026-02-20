# ai-plugins

**AI Plugins** - Curated Claude Code plugins for diverse workflows.

A marketplace of specialized plugins covering various domains:
- **Creative workflows** (Music Production)
- **Productivity & alignment** (Session Goal Tracking)
- **Business innovation** (Business Model Canvas)
- **Finance & compliance** (Dienstreisen / Business Travel)

Each plugin follows a tool-agnostic philosophy: all workflows work with plain Markdown in any editor (Obsidian, VS Code, Notion). Claude Code skills are optional helpers, never requirements.

This is a monorepo containing the plugin marketplace and all plugin source code.

---

## Installation

### Claude Code

Add this marketplace, then install individual plugins:

```bash
/plugin marketplace add ZUHcYa/ai-plugins
```

### Install a Plugin

```bash
/plugin install compass@ai-plugins
/plugin install music-production@ai-plugins
/plugin install knvs@ai-plugins
/plugin install trips@ai-plugins
```

---

## Available Plugins

| Plugin | Description | Status |
|--------|-------------|--------|
| [**compass**](compass/) | Session Goal Alignment - Vision tracking and drift detection | Stable |
| [**music-production**](music-production/) | Music Production Workflow Companion | Stable |
| [**knvs**](knvs/) | Business Model Innovation - Testing Business Ideas with Claude Code | Stable |
| [**trips**](trips/) | Business Travel Tracker - Record Dienstreisen and generate Finanzamt reports | Stable |

See each plugin's README for details:
- [compass/README.md](compass/README.md)
- [music-production/README.md](music-production/README.md)
- [knvs/README.md](knvs/README.md)
- [trips/README.md](trips/README.md)

---

## Repository Structure

```
ai-plugins/
├── .claude-plugin/
│   └── marketplace.json      # Plugin registry (marketplace)
├── compass/                  # Session Goal Alignment
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── CLAUDE.md
│   ├── skills/
│   └── hooks/
├── music-production/         # Music Production
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── CLAUDE.md
│   ├── skills/
│   └── hooks/
├── knvs/                     # Business Model Innovation
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── CLAUDE.md
│   └── skills/
├── trips/                    # Dienstreisen-Tracking
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── CLAUDE.md
│   ├── skills/
│   └── hooks/
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
