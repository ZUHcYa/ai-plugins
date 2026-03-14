# journal: Plugin-spezifische Regeln

## Vision

Persoenliches Journal fuer taegliche Reflexionen, Gedanken und Learnings.
Eine Markdown-Datei pro Tag, Eintraege mit Timestamp angehaengt. Einfach, minimal, tool-agnostisch.

### Strikte Regeln

- **Nur Entwicklungs-Support** fuer journal (Code-Gen, Docs, Automation)
- **Keine User-Features** die Endnutzer direkt nutzen wuerden
- **Keine automatischen Deployments** ohne Review

---

## Versioning

`CHANGELOG.md` ist die Single Source of Truth fuer die Version. `plugin.json` wird automatisch
via git pre-commit hook synchronisiert — niemals manuell editieren.

| Aenderungstyp | Version-Bump |
|---------------|-------------|
| Neues Feature, Breaking Change in Skills | Minor (`0.x.0`) |
| Bugfix, Docs-Korrektur | Patch (`0.x.1`) |

Immer einen `## [x.y.z] - YYYY-MM-DD` Eintrag in `CHANGELOG.md` ergaenzen vor dem Commit.

---

## Datenstruktur

### Konfiguration

Eine Datei: `.journal/config.json`

```json
{
  "dataDir": "./entries"
}
```

**Setup:** `/journal:start` fragt nach `dataDir`.

### Datenverzeichnis

```
<dataDir>/
  2026-02-21.md
  2026-02-20.md
  2026-02-19.md
```

Ein flacher Ordner, eine Datei pro Tag (`YYYY-MM-DD.md`).

### Tages-Datei Format

Frontmatter-Felder:
- `date` (YYYY-MM-DD, required) — Tag den die Datei repraesentiert
- `tags` (array, auto) — Gesammelte Topic-Tags aller Eintraege

Eintraege werden durch `### HH:MM` Headings gezaehlt (nicht durch Frontmatter).
Bestehende Dateien mit zusaetzlichen Frontmatter-Feldern (`entries`, `tasks_open`, etc.)
bleiben kompatibel — extra Felder ignorieren, nicht loeschen.

### Eintrag-Format

```markdown
### HH:MM — Topic Tag

Eintrag-Inhalt hier. Freitext, mehrere Absaetze erlaubt.
```

Ohne Topic-Tag: nur `### HH:MM`.

---

## Namespace-Strategie

| Element | Format | Beispiel |
|---------|--------|----------|
| Skills | `/journal:` Prefix | `/journal:start`, `/journal:write` |
| Konfiguration | `.journal/` Ordner | `.journal/config.json` |
| Tages-Dateien | `YYYY-MM-DD.md` | `2026-02-21.md` |

---

## Markdown-Generierung

Nutzer lesen Journal-Dateien in Markdown-Viewern (Obsidian, VS Code, Notion).
Alle generierten Dateien muessen in beiden Kontexten (Mensch + KI) gut funktionieren.

---

## Tool-Agnostisch

Der Nutzer muss das Journal manuell ohne Skills fuehren koennen.
Skills sind Helfer, nicht Voraussetzung. Keine proprietaeren Formate,
keine Features die nur mit spezifischen Tools funktionieren.
