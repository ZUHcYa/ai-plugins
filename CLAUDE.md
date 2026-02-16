# ai-plugins: Claude Code Workflow

## Monorepo-Struktur

Jedes Plugin hat seinen eigenen Ordner im Root (z.B. `compass/`).
Plugin-spezifische Regeln stehen in der jeweiligen `CLAUDE.md` des Plugin-Ordners.

**Plugins:** compass (Session Alignment), music-production (Music Production Lifecycle)
**State-Dirs:** `.compass/`, `.music-production/` (alle gitignored)

## NIH-Check

Vor Plugin-Entwicklung: Offizielle Anthropic-Registries pruefen (official-plugins.md in auto-memory).
Nichts bauen, was offiziell besser existiert.

## Tool-Agnostisch: Harte Restriktion

**Alle Plugins muessen ohne Claude Code funktionieren.** Der Nutzer muss jeden Workflow
manuell in einem beliebigen Markdown-Editor (Obsidian, VS Code, Notion) durchfuehren koennen.

**Erlaubt:** Reines Markdown, YAML-Frontmatter, selbsterklaerende Ordnerstrukturen
**Verboten:** Features die nur mit Claude Code funktionieren, proprietaere Formate, Tool-spezifische Abhaengigkeiten

Skills sind Helfer, nie Voraussetzung. Details in den jeweiligen Plugin-CLAUDE.md.

---

## Git Workflow

### Branch-Strategie

Niemals direkt auf main arbeiten. Bei jeder Aufgabe automatisch Feature-Branch erstellen.

**Prefixe:** `feature/`, `fix/`, `docs/`, `refactor/`
**Format:** kebab-case, max 50 Zeichen

### Commit & Push

- Commit -> SOFORT Push (kein lokaler Commit ohne Push)
- WIP-Commits erlaubt: `WIP: Beschreibung`
- Direct merge zu main erlaubt (kein PR noetig)

**Commit-Format:**
```
<Typ>: <Kurze Beschreibung>

<Optionale detaillierte Beschreibung>

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```

**Typen:** feat, fix, docs, refactor, test, chore, WIP

### Nach Merge

1. Main zu GitHub pushen
2. Erst nach erfolgreichem Push: Feature-Branch loeschen (lokal + remote)
