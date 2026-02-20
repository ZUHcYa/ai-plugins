# compass: Plugin-spezifische Regeln

## Vision

Kritischer Begleiter fuer Session-Alignment. Haelt die Richtung, erkennt Drift, challenged Annahmen.

### Strikte Regeln

- **Nur Entwicklungs-Support** fuer compass (Code-Gen, Docs, Automation)
- **Keine User-Features** die Endnutzer direkt nutzen wuerden
- **Keine automatischen Deployments** ohne Review

### Versionierung

`CHANGELOG.md` ist die einzige Wahrheitsquelle fuer die Version. `plugin.json` wird automatisch
per Git Pre-commit Hook synchronisiert — niemals manuell bearbeiten.

| Aenderungstyp | Version-Bump |
|---------------|-------------|
| Neues Feature, Breaking Change in Skills/Commands | Minor (`0.x.0`) |
| Bugfix, Docs-Korrektur | Patch (`0.x.1`) |

Vor jedem Commit immer einen `## [x.y.z] - YYYY-MM-DD` Eintrag in `CHANGELOG.md` anlegen.

### Warnsignal-Check

**Vor jeder Aufgabe fragen:**
1. Hilft dies bei der **Entwicklung von compass**?
2. Ist es **kein User-Feature** fuer Endnutzer?

**Bei Zweifeln:** SOFORT den Entwickler fragen!

---

## Architektur

Zwei State-Ebenen im Projekt-Root des Users:
- `.compass/VISION.md` - Persistente Vision (sessionuebergreifend)
- `.compass/session.md` - Session-Ziel (wird pro Session neu gesetzt)
- `.compass/rules.md` - Custom Regeln (user-editierbar)

Drei Hook-Typen:
- **SessionStart** - Fragt nach Session-Ziel
- **PreToolUse** (Write/Bash) - Prueft Alignment vor Aktionen
- **Stop** - Erkennt Drift nach Antworten

Hooks sind IMMER Soft/Advisory: warnen, nie blockieren.
PreToolUse muss schnell sein: Quick-Dismiss fuer offensichtlich passende Aktionen.

---

## Namespace-Strategie

| Element | Format | Beispiel |
|---------|--------|----------|
| Skills | `/compass:` Prefix | `/compass:init`, `/compass:focus` |
| Skill-Dateien | `skills/<skill>/SKILL.md` | `skills/init/SKILL.md` |
| Hooks | `hooks/hooks.json` | SessionStart, PreToolUse, Stop |
| Konfiguration | `.compass/` Ordner | `.compass/VISION.md`, `.compass/session.md` |

### Konfliktvermeidung

- State-Dateien in `.compass/` (versteckter Ordner, gitignored)
- Klare Abgrenzung durch `/compass:` Namespace
- Keine generischen Dateinamen ausserhalb von `.compass/`

---

## Markdown-Generierung

**WICHTIG:** Nutzer lesen State-Dateien in Markdown-Viewern UND Claude Code verarbeitet sie.

**Zielgruppe:**
- **Primaer:** Entwickler/Nutzer (Menschen mit Markdown-Viewer)
- **Sekundaer:** Claude Code / KI (muss Inhalte parsen koennen)

Alle State-Dateien sind reines Markdown mit YAML-Frontmatter.

---

## Emoji-Nutzung

**Regel:** Emojis nur wenn sie echten Mehrwert bieten - nicht als Dekoration.

### Erlaubt
- Status-Indikatoren wo Text allein unklar waere

### Verboten
- Dekorative Emojis ohne Funktion
- Mehrere Emojis pro Zeile/Ueberschrift
- Emojis in Fliesstext
- "AI-Slop" Style

---

## Tool-Agnostisch: Manueller Workflow immer moeglich

**Kernprinzip:** Der Nutzer muss in der Lage sein, VISION.md, session.md und rules.md **manuell ohne Skills** zu lesen und zu bearbeiten.

**Regeln:**
- Alle State-Dateien sind reines Markdown mit Standard-Frontmatter
- Skills sind Helfer, nicht Voraussetzung
- Nutzer koennen Dateien manuell erstellen und bearbeiten
- Ordnerstruktur und Dateiformat sind selbsterklaerend

**Verboten:**
- Proprietaere Dateiformate
- Features, die nur mit Claude Code funktionieren
- Abhaengigkeiten von externen APIs oder Services
