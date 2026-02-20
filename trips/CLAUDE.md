# trips: Plugin-spezifische Regeln

## Vision

Dienstreisen-Tracking fuer die deutsche Steuererklaerung. Reisen erfassen, auflisten
und Finanzamt-taugliche Reports generieren. Kein Lifecycle - reine Erfassung und Auswertung.

Eine Markdown-Datei pro Reise ist das zentrale Format. YAML-Frontmatter ist die Single
Source of Truth, der Markdown-Body bietet menschenlesbare Darstellung plus Notizen.

### Strikte Regeln

- **Nur Entwicklungs-Support** fuer trips (Code-Gen, Docs, Automation)
- **Keine User-Features** die Endnutzer direkt nutzen wuerden
- **Keine automatischen Deployments** ohne Review

### Warnsignal-Check

**Vor jeder Aufgabe fragen:**
1. Hilft dies bei der **Entwicklung von trips**?
2. Ist es **kein User-Feature** fuer Endnutzer?

**Bei Zweifeln:** SOFORT den Entwickler fragen!

---

## Datenstruktur

### Konfiguration

Eine Datei: `.trips/config.json` (nur shared/team-Daten)

```json
{
  "dataDir": "O/5 Support Processes/53 Backoffice/Auslagen",
  "company": "Acme GmbH"
}
```

**Setup:** `/trips:start` fragt `dataDir` und `company` ab. Projekt-CLAUDE.md kann
`## Trips Defaults` mit beiden Werten bereitstellen - dann ist kein Setup noetig.

### Mitarbeitername

Kommt aus dem Claude-Kontext (User Preferences / `~/.claude/CLAUDE.md`), nicht aus der Config.
Falls der Name nicht aus dem Kontext bekannt ist: den User fragen.

### Datenverzeichnis (vom User konfiguriert)

```
<dataDir>/
  2026/
    01 - 2026/
      Friedrich Wagner/
        2026-01-15_berlin.md
        2026-01-20_muenchen.md
        _report-2026-01.md
        _report-2026-01.pdf
    02 - 2026/
      Friedrich Wagner/
        2026-02-03_berlin.md
```

### Pfad-Konstruktion

Alle Reisedateien und Reports leben im Mitarbeiter-Ordner:

```
<dataDir>/<YYYY>/<MM - YYYY>/<FirstName LastName>/
```

- `<YYYY>`: 4-stelliges Jahr
- `<MM - YYYY>`: Zweistelliger Monat + ` - ` + Jahr (z.B. `01 - 2026`). Fuehrende Null.
- `<FirstName LastName>`: Aus Claude-Kontext (User Preferences), Originalschreibweise.
- Ordner erstellen falls noetig (mkdir -p Aequivalent).

`dataDir` ist relativ zum Workspace-Root (cwd). Forward Slashes verwenden.

### Pfad-Handling

- **Bash-Befehle:** Pfade IMMER quoten (`"pfad mit leerzeichen"`)
- **Write/Read/Edit/Glob:** Handlen Leerzeichen korrekt, kein Quoting noetig
- Nach Write-Aufruf NICHT per Bash/ls verifizieren (falsche "nicht gefunden"-Meldungen)

### Reisedatei-Format

YAML-Frontmatter = Single Source of Truth. Body = Tabelle + Notizen.

Felder pro Reise:
- name (string, required) - Kurze Beschreibung der Reise
- startDate (YYYY-MM-DD, required) - Startdatum
- endDate (YYYY-MM-DD, required) - Enddatum
- destination (string, required) - Zielort
- purpose (string, required) - Geschaeftlicher Zweck
- startTime (HH:MM, optional) - Abfahrtszeit
- endTime (HH:MM, optional) - Rueckkunftszeit
- distanceKm (number, optional) - Gefahrene Kilometer
- reimbursementEur (number, optional) - Km-Erstattung (distanceKm x 0,30)
- created (YYYY-MM-DD, auto) - Erstellungsdatum

### Berechnungen

**Kilometerpauschale:** distanceKm x 0,30 EUR/km (§ 9 Abs. 1 Satz 3 Nr. 4a EStG).
Rechenweg immer sichtbar im Body: `580 km x 0,30 EUR/km = 174,00 EUR`.
Ergebnis als `reimbursementEur` im YAML gespeichert.

**Reisetage:** Kalendertage von startDate bis endDate inklusive.
Gleicher Tag = 1 Reisetag. 10.02. bis 12.02. = 3 Reisetage.

### Dateibenennungsregel

`YYYY-MM-DD_<ziel-kebab-case>.md`

Beispiele:
- `2026-02-03_berlin.md`
- `2026-01-20_muenchen.md`
- `2026-03-15_frankfurt-am-main.md`

Regeln: Kleinbuchstaben, keine Umlaute (ue statt ue), Bindestriche statt Leerzeichen.

### Report-Dateien

`.md` ist Source of Truth. `.pdf` ist generiertes Exportformat (via MCP, siehe unten).

| Scope | Markdown | PDF | Ablageort |
|-------|----------|-----|-----------|
| Monatsreport | `_report-YYYY-MM.md` | `_report-YYYY-MM.pdf` | `<dataDir>/<YYYY>/<MM - YYYY>/<Name>/` |

---

## MCP-Server: markdown2pdf

Das Plugin bringt einen MCP-Server mit (`trips/.mcp.json`), der automatisch startet
wenn das Plugin aktiviert ist. Er konvertiert Markdown-Reports zu PDF.

**Server:** `markdown2pdf-mcp` (Node.js + Puppeteer, on-demand via npx)
**Tool:** `create_pdf_from_markdown`
**Verwendung:** Automatisch durch `/trips:report` nach dem Speichern der .md-Datei.

MCP ist ein Automatisierungslayer - kein Kernbestandteil (siehe ADR-007).
Ohne MCP bleibt der Markdown-Report vollstaendig nutzbar.

**WICHTIG:** `outputFilename` akzeptiert NUR Dateinamen (z.B. `_report-2026-02.pdf`), keine Pfade.
Das Tool speichert in `M2P_OUTPUT_DIR` (default: HOME). PDF danach in den Zielordner verschieben.

**Voraussetzungen fuer PDF-Export:**
- Node.js 18+ und npm (fuer npx)
- Puppeteer laedt beim ersten Start ~170 MB Chromium herunter
- Ohne Node.js: PDF-Export wird uebersprungen, alle anderen Features funktionieren normal

**Version-Pflege:** Version ist in `.mcp.json` gepinnt. Bei Bedarf pruefen:
`npm view markdown2pdf-mcp version` -- bei neuer Version `.mcp.json` aktualisieren.

---

## Namespace-Strategie

| Element | Format | Beispiel |
|---------|--------|----------|
| Skills | `/trips:` Prefix | `/trips:start`, `/trips:new` |
| Skill-Dateien | `skills/<skill>/SKILL.md` | `skills/start/SKILL.md` |
| Konfiguration | `.trips/` Ordner | `.trips/config.json` |
| Reisedateien | `YYYY-MM-DD_ziel.md` | `2026-02-03_berlin.md` |

### Konfliktvermeidung

- State-Dateien in `.trips/` (versteckter Ordner, gitignored)
- Klare Abgrenzung durch `/trips:` Namespace
- Datenverzeichnis vom User frei waehlbar

---

## Markdown-Generierung

**WICHTIG:** Nutzer und Steuerberater lesen Reisedateien in Markdown-Viewern (Obsidian,
VS Code) UND Claude Code verarbeitet sie.

**Zielgruppe:**
- **Primaer:** Nutzer / Steuerberater (Menschen mit Markdown-Viewer)
- **Sekundaer:** Claude Code / KI (muss YAML-Frontmatter parsen koennen)

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

**Kernprinzip:** Der Nutzer muss immer in der Lage sein, Dienstreisen **manuell ohne
Skills** zu erfassen. Das gewaehlte Tool (Obsidian, VS Code, etc.) darf keine Rolle spielen.

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
