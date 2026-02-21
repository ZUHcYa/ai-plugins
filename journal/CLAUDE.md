# journal: Plugin-spezifische Regeln

## Vision

Persoenliches Journal fuer taegliche Reflexionen, Gedanken und Learnings.
Eine Markdown-Datei pro Tag, Eintraege mit Timestamp angehaengt. Einfach, minimal, tool-agnostisch.

YAML-Frontmatter ist die Single Source of Truth fuer strukturierte Daten (Anzahl Eintraege, Tags, Tasks).
Der Markdown-Body enthaelt die eigentlichen Journal-Eintraege in chronologischer Reihenfolge.

### Strikte Regeln

- **Nur Entwicklungs-Support** fuer journal (Code-Gen, Docs, Automation)
- **Keine User-Features** die Endnutzer direkt nutzen wuerden
- **Keine automatischen Deployments** ohne Review

### Warnsignal-Check

**Vor jeder Aufgabe fragen:**
1. Hilft dies bei der **Entwicklung von journal**?
2. Ist es **kein User-Feature** fuer Endnutzer?

**Bei Zweifeln:** SOFORT den Entwickler fragen!

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
  "dataDir": "./entries",
  "filePattern": "YYYY-MM-DD",
  "taskManagement": "inline",
  "taskSystemRef": null
}
```

**Setup:** `/journal:start` fragt nach `dataDir`, `filePattern` und `taskManagement`.

### Datenverzeichnis (nutzerkonfiguriert)

```
<dataDir>/
  2026-02-21.md
  2026-02-20.md
  2026-02-19.md
```

Ein flacher Ordner, eine Datei pro Tag. Einfach und greppbar.

### Tages-Datei Format

Frontmatter = strukturierte Metadaten. Body = Eintraege mit Timestamp.

Frontmatter-Felder:
- `date` (YYYY-MM-DD, required) — Tag den die Datei repraesentiert
- `entries` (number, auto) — Anzahl Eintraege in dieser Datei
- `tags` (array, auto) — Gesammelte Topic-Tags aller Eintraege
- `tasks_open` (number, auto) — Offene Tasks (nur bei `taskManagement: "inline"`)
- `tasks_done` (number, auto) — Erledigte Tasks (nur bei `taskManagement: "inline"`)
- `created` (YYYY-MM-DD, auto) — Erstellungsdatum

### Eintrag-Format (in der Tages-Datei)

```markdown
### HH:MM — Topic Tag

Eintrag-Inhalt hier. Freitext, mehrere Absaetze erlaubt.

- [ ] Optionale Aufgabe (nur bei inline Task-Management)
```

Ohne Topic-Tag: nur `### HH:MM`.

---

## Namespace-Strategie

| Element | Format | Beispiel |
|---------|--------|----------|
| Skills | `/journal:` Prefix | `/journal:start`, `/journal` |
| Skill-Dateien | `skills/<skill>/SKILL.md` | `skills/start/SKILL.md` |
| Konfiguration | `.journal/` Ordner | `.journal/config.json` |
| Tages-Dateien | `YYYY-MM-DD.md` | `2026-02-21.md` |

### Konfliktvermeidung

- State-Dateien in `.journal/` (versteckter Ordner, gitignored)
- Klare Abgrenzung durch `/journal:` Namespace
- Datenverzeichnis frei konfigurierbar

---

## Markdown-Generierung

**WICHTIG:** Nutzer lesen Journal-Dateien in Markdown-Viewern (Obsidian, VS Code, Notion)
UND Claude Code liest die Inhalte.

**Zielgruppe:**
- **Primaer:** Nutzer (Mensch mit Markdown-Viewer)
- **Sekundaer:** Claude Code / KI (muss YAML-Frontmatter und Eintraege parsen koennen)

Alle generierten Dateien muessen in beiden Kontexten gut funktionieren.

---

## Emoji-Nutzung

**Regel:** Emojis nur wenn sie echten Mehrwert bieten — nicht als Dekoration.

### Erlaubt
- Status-Indikatoren wo Text allein unklar waere

### Verboten
- Dekorative Emojis ohne Funktion
- Mehrere Emojis pro Zeile/Ueberschrift
- Emojis in Fliesstext
- "AI-Slop" Style

---

## Tool-Agnostisch: Manueller Workflow immer moeglich

**Kernprinzip:** Der Nutzer muss immer in der Lage sein, das Journal **manuell ohne Skills** zu fuehren.
Das gewaehlte Tool (Obsidian, VS Code, etc.) darf keine Rolle spielen.

**Regeln:**
- Keine Features bauen, die nur mit spezifischen Tools funktionieren
- Alle Dateien sind reines Markdown mit Standard-Frontmatter
- Skills sind Helfer, nicht Voraussetzung
- Nutzer koennen Dateien manuell erstellen und bearbeiten
- Ordnerstruktur und Dateiformat sind selbsterklaerend

**Verboten:**
- Obsidian-spezifische Plugins oder Dataview-Queries als Kernfunktion
- Proprietaere Dateiformate
- Features, die nur mit Claude Code funktionieren
- Abhaengigkeiten von externen APIs oder Services
