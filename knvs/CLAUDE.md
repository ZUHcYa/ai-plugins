# knvs: Plugin-spezifische Regeln

## Vision

**knvs** ist ein Template fuer Innovation Manager basierend auf dem Strategyzer "Testing Business Ideas" Prozess.

### Strikte Regeln

- **Nur Entwicklungs-Support** fuer knvs (Code-Gen, Docs, Tests, Automation)
- **Keine User-Features** die Innovation Manager direkt nutzen wuerden
- **Keine automatischen Deployments** ohne Review

### Warnsignal-Check

**Vor jeder Aufgabe fragen:**
1. Hilft dies bei der **Entwicklung von knvs**?
2. Ist es **kein User-Feature** fuer knvs-Endnutzer?

**Bei Zweifeln:** SOFORT den Entwickler fragen!

---

## Markdown-Generierung

**WICHTIG:** Innovation Manager nutzen Markdown-Viewer (Obsidian, Notion, VS Code) UND Claude Code liest die Inhalte.

**Zielgruppe:**
- **Primaer:** Innovation Manager (Menschen mit Markdown-Viewer)
- **Sekundaer:** Claude Code / KI (muss Inhalte parsen koennen)

Alle generierten Dateien muessen in beiden Kontexten gut funktionieren.

---

## Dateinamen-Konventionen

| Typ | Format | Beispiel |
|-----|--------|---------|
| Canvas (alle Phasen) | `[slug].md` | `ai-bookkeeping.md` |
| Research Report | `[slug].md` | `machine-customers-verified.md` |
| Impact Atom | `[slug].md` | `vector-search-mandatory.md` |
| Network Canvas | `[entity-slug].md` | `vendor-corp.md` |

**Regel:** KEIN Datumspräfix. KEINE Nummerierung. Nur kebab-case Slug.
CORRECT: `ai-bookkeeping-app.md` — WRONG: `20260220-001-ai-bookkeeping-app.md`

---

## Business Model Canvas: Pflichtfelder

Jeder Canvas MUSS diese 9 Kernfelder enthalten (Osterwalder/Pigneur), als `##` Headings:

- **Value Proposition** - Welchen Wert liefern wir? Welches Problem loesen wir?
- **Customer Segments** - Fuer wen schaffen wir Wert?
- **Channels** - Wie erreichen wir unsere Kunden?
- **Customer Relationships** - Welche Beziehung erwartet jedes Segment?
- **Revenue Streams** - Wofuer sind Kunden bereit zu zahlen?
- **Key Resources** - Welche Ressourcen braucht unser Wertversprechen?
- **Key Activities** - Welche Aktivitaeten braucht unser Wertversprechen?
- **Key Partnerships** - Wer sind unsere wichtigsten Partner und Lieferanten?
- **Cost Structure** - Was sind die wichtigsten Kosten?

**Fehlende Felder = ungueltiger Canvas.** `/knvs:sync` prueft dies.

**Impact Callouts (optional):** Unter jedem BMC-Feld koennen Impact Atoms als inline Callouts eingebettet werden. Format: Obsidian-Callout-Syntax `> [!impact]`. Siehe `skills/ideate/SKILL.md` fuer Details.

### Phasen-spezifische Erweiterungen

Zusaetzlich zu den 9 Kernfeldern kommen pro Phase weitere Pflicht-Elemente hinzu:

- **IDEATE:** Frontmatter `status`, `progress`, `created`, `updated`. Sections: Notes, Next Steps
- **EXPLORE:** Frontmatter zusaetzlich `innovation_risk`, `potential_revenue`. Sections: Hypotheses, Experiments
- **EXPLOIT:** Frontmatter zusaetzlich `next_review`, `disruption_risk`, `revenue_score`. Sections: Reviews

Kanonische Templates fuer jede Phase stehen in den jeweiligen `skills/*/SKILL.md` unter `## Canvas Template`.

---

## Research Report: Struktur

Forschungsberichte in `research/` sind PRE-IDEATE Dokumente. Sie sind KEINE Business Model Canvases.

**Pflichtfelder (Frontmatter):**
- `type: research`
- `title`
- `status` (hypothesis | draft | verified)
- `created`
- `verified` (YYYY-MM-DD, wenn status=verified)

**Optionale Felder:**
- `source_report` (Referenz auf Originaldokument)
- `vendor` (Name der externen Organisation, wenn Report auf einen konkreten Vendor, Partner oder Wettbewerber fokussiert)

**Hinweis:** `status: verified` kann manuell gesetzt werden, wenn der Forschungsbericht
bereits extern geprüft wurde. Das `verified`-Datumsfeld ist auch bei manueller Verifizierung
Pflicht (eigenes Prüfdatum eintragen).

**Required Sections:**
- Zusammenfassung
- Forschungskontext (Problemraum, Marktkontext, Zielgruppe)
- Kernbefunde (mit Quellen)
- Stuetzende Daten
- Implikationen fuer Geschaeftsmodell
- Naechste Schritte

**Optionale Sections:**
- Limitationen
- Quellenverzeichnis

Research Reports werden via `/knvs:impact` zu Impact Atoms und via `/knvs:ideate` zu BMC Canvases transformiert.

### Research Report Template

```markdown
---
type: research
title: "[Forschungsthema]"
status: verified
created: YYYY-MM-DD
verified: YYYY-MM-DD
source_report: "[Originaldateiname]"  # optional
---

# Forschungsbericht: [Titel]

## Zusammenfassung

[Ueberblick ueber die Forschungsergebnisse]

---

## Forschungskontext

### Problemraum
[Welches Problem adressiert diese Forschung?]

### Marktkontext
[Marktgroesse, Trends, Dynamiken]

### Zielgruppe
[Wer ist von diesem Problem betroffen?]

---

## Kernbefunde

### Befund 1: [Titel]
[Inhalt]

**Quellen:**
- [Zitat 1 mit Link]

---

## Stuetzende Daten

### Marktstatistiken
[Statistiken mit Quellen]

### Wettbewerbslandschaft
[Wettbewerbsanalyse]

### Technische Machbarkeit
[Technische Einschaetzungen]

---

## Implikationen fuer Geschaeftsmodell

[Einschaetzungen wie diese Forschung ein Geschaeftsmodell informieren koennte]

Potenzielle BMC-Felder:
- **Value Proposition:** [Aus Problemraum und Befunden]
- **Customer Segments:** [Aus Zielgruppenanalyse]
- **Revenue Streams:** [Aus Marktkontext]

---

## Naechste Schritte

- [ ] /knvs:impact um Impact Atoms zu extrahieren
- [ ] /knvs:ideate fuer Business Model Canvas
```

### Hypothese (Research-Vorlaeuferstufe)

Hypothesen sind UNBESTAETIGTE BEHAUPTUNGEN die waehrend der Arbeit entstehen.
Sie sind Recherche-Auftraege, keine Fakten.

**Wann erfassen?**
Wenn der User eine Behauptung trifft, die:
- Nicht aus verifizierter Research stammt
- Business-Model-Entscheidungen beeinflusst (z.B. Severity, Canvas-Design)
- Validierung durch externe Recherche braucht

Jeder Skill, der auf eine unbestaetigte Marktannahme stoesst, bietet
Hypothesen-Erfassung an. Der User kann Hypothesen auch manuell anlegen.

**Pflichtfelder (Frontmatter):**
- `type: research`
- `title`
- `status: hypothesis`
- `created`
- `claim` (Einzeiler: die zu pruefende Behauptung)
- `bmc_fields` (Array betroffener BMC-Felder, kanonische englische Namen)

**Optionale Felder:**
- `origin_impact` (Pfad zum Impact Atom, bei Severity-Dispute)

**Required Sections:**
- Behauptung (erweitert claim mit Kontext und Nuancen)
- Kontext (Entstehung, betroffener Canvas, Severity-Entscheidung)
- Recherche-Auftrag (Checkliste konkreter Aufgaben)

Hypothesen enthalten KEINE Befunde, Quellen oder Korrekturprotokoll.

**Lebenszyklus:**

| Ergebnis | Aktion |
|----------|--------|
| Bestaetigt | Research in research/ mit status: verified ablegen -> /knvs:impact -> Hypothese loeschen |
| Widerlegt | Severity im Canvas korrigieren -> Hypothese loeschen |
| Nicht relevant | Hypothese loeschen |
| Nicht recherchiert (>14 Tage) | /knvs:sync warnt |

Hypothesen sind working documents. Nach Recherche oder Verwerfung loeschen.
Kanonisches Template steht in `skills/ideate/SKILL.md` unter `## Hypothese-Template`.

---

## Impact Atom: Struktur

Impact Atoms in `impacts/` sind PRE-IDEATE Dokumente. Sie stehen zwischen Research und BMC.
Jeder Impact beschreibt EINEN spezifischen Druck oder eine Chance fuer Geschaeftsmodelle.

**Pflichtfelder (Frontmatter):**
- `type: impact-atom` (fest)
- `title`
- `tags` (Array, Kategorisierung)
- `driver` (Wiki-Link zu Research, oder Array bei mehreren Quellen)
- `bmc_fields` (Array der betroffenen BMC-Felder, kanonische englische Namen)
- `created` (YYYY-MM-DD)

**Required Sections:**
- Titel mit `[IMPACT]` Prefix
- Das Szenario (Was passiert?)
- Der Druck auf Geschaeftsmodelle (Bullet pro betroffenem BMC-Feld)
- Quelle (Wiki-Link zum Research-Report)

**Regeln:**
- Impact Atoms sind nach Erstellung unveraenderlich (immutable)
- **Keine Severity im Atom.** Severity wird kontextspezifisch bei Canvas-Einbettung (`/knvs:ideate`) vergeben.
- Dateiname: `<impact-slug>.md`
- BMC-Feldnamen muessen exakt den 9 kanonischen Namen entsprechen
- Multi-Driver: `driver` kann Array sein bei gleicher Erkenntnis aus mehreren Quellen

Kanonisches Template steht in `skills/impact/SKILL.md` unter `## Impact Atom Template`.
Impact Atoms werden via `/knvs:ideate` als inline Callouts in BMC Canvases eingebettet und via `/knvs:review` als Challenge-Grundlage genutzt.

---

## Network Canvas: Struktur

Network Canvases in `network/` bilden Geschäftsmodelle externer Parteien ab (Vendors,
Partner, Kunden, Wettbewerber). Sie dienen als **Heimatort für indirekte Impacts** - Impacts
die primär das externe Modell betreffen und erst sekundär auf unsere Canvases wirken.

**Warum getrennt?** Beim Betrachten unseres Canvas sollen externe Impacts nicht als
vollständige `[!impact]`-Callouts erscheinen. Stattdessen zeigt unser Canvas nur einen
leichtgewichtigen `[!vendor-risk]`-Verweis zum Network Canvas.

**Pflichtfelder (Frontmatter):**
- `type: network-canvas` (fest)
- `entity` (offizieller Name der externen Organisation)
- `relationship` (vendor | partner | customer | competitor | industry)
- `snapshot_date` (YYYY-MM-DD - wann war dieser Stand aktuell?)

**Optionale Felder:**
- `product` (Produkt/Plattform wenn relevant, z.B. "STEP")
- `source_research` (Wiki-Link zum Research Report der Grundlage war)

**BMC-Felder:** Nur die analytisch relevanten Felder befüllen - kein Zwang zu allen 9.
Impact-Callouts werden wie im eigenen Canvas eingebettet (`> [!impact]`), beschreiben
aber den Druck auf das EXTERNE Modell, nicht auf unseres.

**Kein Lifecycle:** Keine IDEATE/EXPLORE/EXPLOIT-Phasen, kein `next_review`, kein
`innovation_risk`. Network Canvases sind Analyse-Snapshots, keine Innovation-Objekte.

**Staleness:** `/knvs:sync` warnt wenn `snapshot_date` älter als 90 Tage ist.

### Network Canvas Template

```markdown
---
type: network-canvas
entity: "Vendor Corp AG"
product: "PlatformX"
relationship: vendor
snapshot_date: YYYY-MM-DD
source_research: "[[research/vendor-corp-analyse]]"  # optional
---

# Vendor Corp/PlatformX - Vendor Model Snapshot

## Value Proposition

> [!impact]- KI-gestützte ERP-Alternativen setzen VP unter Druck (HIGH)
> **Driver:** [[research/vendor-corp-analyse]]
>
> GenAI-native Konkurrenten bieten vergleichbare ERP-Plattform-Funktionalität...
>
> _Quelle: [[research/vendor-corp-analyse]]_

[Was Vendor Corp anbietet - nur bekannte / relevante Aspekte]

## Key Resources
[Was Vendor Corp als Key Resource hat]

## Revenue Streams
[Wie Vendor Corp verdient - relevant für unsere Cost Structure]
```

### `[!vendor-risk]`-Callout im eigenen Canvas

Wenn unser Canvas einen Vendor/Partner referenziert der in `network/` dokumentiert ist,
wird ein `[!vendor-risk]`-Callout direkt unter dem betroffenen BMC-Feld eingefügt:

```markdown
## Key Partnerships

> [!vendor-risk]- Vendor Corp/PlatformX: Vendor aktuell unter Druck ([[network/vendor-corp]])
> Aktuelle Situation: 2 HIGH, 1 MEDIUM Impacts auf Vendor-Modell.
> Risiko: Planungssicherheit in diesem Bereich eingeschränkt.

**Vendor Corp/PlatformX** ist unser primärer ERP-Plattform-Provider...
```

`[!vendor-risk]` ist kein Impact Atom - es ist ein Referenz-Indikator. Die Impacts
selbst leben im Network Canvas, nicht in `impacts/` und nicht direkt in unserem Canvas.

---

## Namespace-Strategie

| Element | Format | Beispiel |
|---------|--------|----------|
| Skills | `/knvs:` Prefix | `/knvs:impact`, `/knvs:ideate`, `/knvs:start` |
| Skill-Dateien | `<skill>/SKILL.md` | `ideate/SKILL.md`, `start/SKILL.md` |
| Konfiguration | `.knvs/` Ordner | `.knvs/config.json` |
| Phasen-Ordner | Kurz, eindeutig | `research/`, `impacts/`, `ideate/`, `explore/`, `exploit/` |

### Konfliktvermeidung

- Keine generischen Dateinamen (`config.json`, `settings.md`)
- Keine Ueberschreibung von Standard-Dateien
- Klare Abgrenzung durch Prefixes/Namespaces
- Phasen-Ordner sind kurz aber spezifisch genug

---

## Emoji-Nutzung

**Regel:** Emojis nur wenn sie echten Mehrwert bieten - nicht als Dekoration.

### Erlaubt
- Status-Indikatoren wo Text allein unklar waere (z.B. in kompakten Tabellen)
- Phasen-Symbole zur schnellen visuellen Orientierung

### Verboten
- Dekorative Emojis ohne Funktion
- Mehrere Emojis pro Zeile/Ueberschrift
- Emojis in Fliesstext
- "AI-Slop" Style (uebertriebene Emoji-Nutzung)

---

## Tool-Agnostisch: Manueller Workflow immer moeglich

**Kernprinzip:** Der Nutzer muss immer in der Lage sein, den knvs-Prozess **manuell ohne Skills** durchzufuehren. Das gewaehlte Markdown-Tool (Obsidian, VS Code, Notion, etc.) darf keine Rolle spielen.

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
