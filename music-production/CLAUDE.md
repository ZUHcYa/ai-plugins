# music-production: Plugin-spezifische Regeln

## Vision

Begleitung des Musikproduktionsprozesses mit Skills - von der Idee bis zum fertigen Track.
Die Song-Datei (`<Ordnername>.md`) ist das zentrale Begleitdokument und waechst mit dem Song durch alle Phasen.

### Strikte Regeln

- **Nur Entwicklungs-Support** fuer music-production (Code-Gen, Docs, Automation)
- **Keine User-Features** die Musiker direkt nutzen wuerden
- **Keine automatischen Deployments** ohne Review

### Warnsignal-Check

**Vor jeder Aufgabe fragen:**
1. Hilft dies bei der **Entwicklung von music-production**?
2. Ist es **kein User-Feature** fuer Endnutzer?

**Bei Zweifeln:** SOFORT den Entwickler fragen!

---

## Song Lifecycle

```
01_Projects/          ->  02_Vault/       ->  Release-Prep        ->  03_Catalog/
Produktion               Fertig, wartet      Texte & Meta             Veroeffentlicht

/music-production:new    :vault              :release                 :catalog
erstellt                 verschiebt          reichert an              verschiebt &
<Ordnername>.md          + status update     + generiert              nummeriert

                                             :generate
                                             Release-Texte
                                             aus Beschreibung
```

**Zentrales Artefakt:** `<Ordnername>.md` (z.B. `2026-02-Nightfall.md`, nach Catalog: `FW004-Nightfall.md`) - waechst von minimalen Notizen (Production) zu vollstaendigem Release-Dokument (Catalog).

---

## Namespace-Strategie

| Element | Format | Beispiel |
|---------|--------|----------|
| Skills | `/music-production:` Prefix | `/music-production:start`, `/music-production:new` |
| Skill-Dateien | `skills/<skill>/SKILL.md` | `skills/start/SKILL.md` |
| Konfiguration | `.music-production/` Ordner | `.music-production/config.json` |
| Song-Dokument | `<Ordnername>.md` | Datei heisst wie der Ordner (z.B. `2026-02-Nightfall.md`) |

### Konfliktvermeidung

- Keine generischen Dateinamen (`config.json`, `settings.md`)
- Klare Abgrenzung durch `.music-production/` Namespace
- Song-Datei heisst wie der Ordner - eindeutig und sofort erkennbar

---

## Markdown-Generierung

**WICHTIG:** Musiker nutzen Markdown-Viewer (Obsidian, VS Code) UND Claude Code liest die Inhalte.

**Zielgruppe:**
- **Primaer:** Musiker (Menschen mit Markdown-Viewer)
- **Sekundaer:** Claude Code / KI (muss Inhalte parsen koennen)

Alle generierten Dateien muessen in beiden Kontexten gut funktionieren.

---

## Emoji-Nutzung

**Regel:** Emojis nur wenn sie echten Mehrwert bieten - nicht als Dekoration.

### Erlaubt
- Status-Indikatoren wo Text allein unklar waere (z.B. in kompakten Tabellen)

### Verboten
- Dekorative Emojis ohne Funktion
- Mehrere Emojis pro Zeile/Ueberschrift
- Emojis in Fliesstext
- "AI-Slop" Style

---

## Tool-Agnostisch: Manueller Workflow immer moeglich

**Kernprinzip:** Der Nutzer muss immer in der Lage sein, den Produktionsprozess **manuell ohne Skills** durchzufuehren. Das gewaehlte Tool (Obsidian, VS Code, etc.) darf keine Rolle spielen.

**Regeln:**
- Keine Features bauen, die nur mit spezifischen Tools funktionieren
- Alle Dateien sind reines Markdown mit Standard-Frontmatter
- Skills sind Helfer, nicht Voraussetzung
- Nutzer koennen Dateien manuell erstellen, verschieben, bearbeiten
- Ordnerstruktur und Dateiformat sind selbsterklaerend

**Verboten:**
- Obsidian-spezifische Plugins oder Dataview-Queries als Kernfunktion
- Proprietaere Dateiformate
- Features, die nur mit Claude Code funktionieren
- Abhaengigkeiten von externen APIs oder Services
