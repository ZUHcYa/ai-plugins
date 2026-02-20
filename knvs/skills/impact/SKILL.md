---
name: "impact"
description: "Extract atomic impacts from research for BMC pressure analysis"
---

# /knvs:impact

## Purpose

Extrahiert atomare Auswirkungen (Impact Atoms) aus Forschungsberichten.
Jeder Impact beschreibt einen spezifischen Druck oder eine Chance fuer Geschaeftsmodelle.
Impact Atoms sind die Bruecke zwischen Forschung und Business Model Canvas.

## When to Use

- Nach Einlesen eines Forschungsberichts in `research/`
- Vor der BMC-Erstellung (`/knvs:ideate`)
- Wenn systematisch alle Geschaeftsmodell-Implikationen einer Forschung erfasst werden sollen

## Workflow

### Single Research Paper

```
User: /knvs:impact
Claude: Welchen verifizierten Forschungsbericht moechtest du analysieren?

        Verfuegbare Reports (research/):
        1. Machine Customers Analysis [VERIFIED]
        2. AI Commerce Trends [VERIFIED]
        3. Fitness Market Study [DRAFT]
        4. Vendor X KI-Wandel [HYPOTHESIS] (nicht verfuegbar)

User: 1
Claude: Analysiere research/machine-customers-verified.md...

        Ich habe 4 potenzielle Impacts identifiziert:

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 1 von 4
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Titel: Erwartung: Semantische Vektoren statt Bilder
        Tags: [tech-shift, ai-readiness, market-pressure]
        BMC-Felder: Value Proposition, Key Resources

        Das Szenario:
        Kuenftige Kaeufer (KI-Agenten) ignorieren reine HTML- und Bild-Daten.
        Sie benoetigen Vektoren und strukturierte Attribute.

        Der Druck auf Geschaeftsmodelle:
        * Value Prop: Systeme, die nur "Visuals" liefern, werden unsichtbar.
        * Key Resources: Vektor-Datenbanken und Schema.org-Kompetenz werden Pflicht.

        -> Uebernehmen? [J/n/bearbeiten]

User: J
Claude: Impact 1 gespeichert.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 2 von 4
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Titel: Preisvergleich wird algorithmisch
        Tags: [market-pressure, pricing]
        BMC-Felder: Revenue Streams, Channels
        [...]

        -> Uebernehmen? [J/n/bearbeiten]

User: n
Claude: Impact 2 uebersprungen.

        [... weiter fuer verbleibende Impacts ...]

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        ZUSAMMENFASSUNG
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Analysiert: research/machine-customers-verified.md
        Vorgeschlagen: 4
        Uebernommen: 3
        Uebersprungen: 1

        Gespeicherte Dateien:
        - impacts/machine-customers--vector-search-mandatory.md
        - impacts/machine-customers--schema-org-pflicht.md
        - impacts/machine-customers--agent-negotiation.md

        Naechste Schritte:
        - /knvs:impact fuer weitere Research-Reports
        - /knvs:ideate um BMC mit Impact-Kontext zu erstellen
```

### Alle unverarbeiteten Reports

```
User: /knvs:impact (dann: "analysiere alle")
Claude: Scanne research/ nach Reports ohne Impacts...

        Unverarbeitete Reports:
        1. AI Commerce Trends [VERIFIED] - 0 Impacts

        Bereits verarbeitete Reports:
        2. Machine Customers [VERIFIED] - 3 Impacts

        Starte mit "AI Commerce Trends"?

User: J
Claude: [... gleicher Batch-Review Flow wie oben ...]
```

---

## What the Skill Does

1. Liest `.knvs/config.json` fuer targetPath
2. Scannt `research/` nach Reports mit `status: verified` oder `status: draft` (Hypothesen ausgeschlossen)
3. Wenn kein Argument: listet verfuegbare Reports, User waehlt
4. Wenn User "alle" oder "all" sagt: scannt `driver`-Felder aller Impacts in `impacts/` und findet Reports ohne zugehoerige Atoms
5. Liest den ausgewaehlten Forschungsbericht vollstaendig
6. Identifiziert distinkte Impacts:
   - Jeder Impact = ein spezifischer Druck ODER eine Chance
   - Muss mindestens ein BMC-Feld betreffen
   - Muss aus dem Forschungsinhalt ableitbar sein (keine Spekulation)
7. Prueft auf Duplikate: Vergleicht Titel und `bmc_fields` mit existierenden Atoms in `impacts/`
   - Bei Treffer: Zeigt existierenden Impact und fragt ob Merge (multi-driver) oder Skip
8. Praesentiert jeden Impact einzeln zur Bestaetigung:
   - Titel, Tags, BMC-Felder, Szenario, Druck
   - User kann: uebernehmen (J), ablehnen (n), oder bearbeiten
   - Bei "bearbeiten": User kann Tags, BMC-Felder anpassen
9. Schreibt bestaetigte Impacts als einzelne .md Dateien in `impacts/`
10. Zeigt Zusammenfassung am Ende

---

## Impact-Erkennung

Der Skill sucht im Forschungsbericht nach:

| Quelle im Report | Impact-Typ |
|-------------------|------------|
| Kernbefunde | Direkter Technologie- oder Marktdruck |
| Implikationen fuer Geschaeftsmodell | Explizite BMC-Auswirkungen |
| Stuetzende Daten / Statistiken | Quantifizierbare Marktverschiebungen |
| Limitationen | Unsicherheiten die trotzdem Handlungsdruck erzeugen |

### Vendor- und Partner-Analyse

Wenn ein Research Report über eine externe Organisation handelt (Vendor, Partner, Kunde,
Wettbewerber), landen die extrahierten Impacts **im Network Canvas der externen Partei**
(`network/<slug>.md`), nicht in `impacts/`.

```
Claude: Dieser Report handelt von "Vendor Corp AG" (Vendor).
        Impacts werden in network/vendor-corp.md eingebettet, nicht in impacts/.

        Impact 1: KI-gestützte Alternativen setzen VP unter Druck
        -> [!impact]-Callout in network/vendor-corp.md, Sektion "## Value Proposition"
```

**Warum?** Impacts auf externe Modelle sollen in unserem Canvas nur als leichtgewichtiger
`[!vendor-risk]`-Verweis erscheinen, nicht als vollständige Impact-Callouts. Der Network
Canvas ist der Heimatort dieser Impacts.

**Erkennungsregel:** Wenn Research `vendor`-Feld gesetzt hat, oder Titel/Inhalt auf eine
konkrete externe Organisation fokussiert, nach Network Canvas fragen:

```
Claude: Dieser Report analysiert "Vendor Corp AG".
        Existiert bereits network/vendor-corp.md?
        [J] Impacts direkt in Network Canvas einbetten
        [N] Network Canvas neu anlegen, dann Impacts einbetten
        [A] Als reguläre Impact Atoms behandeln (impacts/)
```

**Bei `[N]` - Network Canvas erstellen:**

```
Claude: Neuen Network Canvas anlegen für "Vendor Corp AG".

        Beziehung zu diesem Unternehmen?
        [1] vendor    - Lieferant / Technologie-Partner
        [2] partner   - strategischer Partner
        [3] customer  - B2B-Kunde
        [4] competitor - Wettbewerber
        [5] industry  - Branche / Marktsegment

User: 1

Claude: Produkt/Plattform-Name? (Enter für keinen)
User: PlatformX

Claude: Network Canvas erstellt: network/vendor-corp.md
        -> Impacts werden jetzt als [!impact]-Callouts eingebettet...
```

Der erstellte Network Canvas nutzt das Template aus `CLAUDE.md` mit den beim Research
identifizierten BMC-Feldern als Sektionen.

Falls `[A]`: normale Impact-Extraktion nach `impacts/`, Formulation bleibt generisch
("Ein Geschäftsmodell, das [Vendor] als Partner nutzt, ...").

| Quelle im Report | Impact-Typ | Ziel |
|---|---|---|
| Kernbefunde über EIGENEN Markt | Direkter Technologie- oder Marktdruck | `impacts/` |
| Kernbefunde über externen Vendor/Partner | Druck auf deren Geschäftsmodell | `network/<slug>.md` |

### BMC-Feld-Zuordnung

Jeder Impact muss mindestens einem der 9 kanonischen BMC-Felder zugeordnet werden:
- Value Proposition
- Customer Segments
- Channels
- Customer Relationships
- Revenue Streams
- Key Resources
- Key Activities
- Key Partnerships
- Cost Structure

Ein Impact kann mehrere Felder betreffen. Die Zuordnung wird dem User zur Bestaetigung vorgelegt.

Bei ambivalenten Befunden die eher Vorhersage als Fakt sind,
siehe "Hypothese (Research-Vorlaeuferstufe)" in CLAUDE.md.

---

## Duplikat-Erkennung

```
Claude: Aehnlicher Impact existiert bereits:

        Existierend: impacts/machine-customers--vector-search-mandatory.md
        - "Erwartung: Semantische Vektoren statt Bilder"
        - Driver: [[research/machine-customers-verified]]

        Neuer Vorschlag aus ai-commerce-trends:
        - "Vektor-Suche wird Standard-Anforderung"
        - Driver: [[research/ai-commerce-trends-verified]]

        Optionen:
        [M] Merge: Driver zum existierenden Impact hinzufuegen
        [N] Neu: Als separaten Impact speichern
        [S] Skip: Ueberspringen
```

Bei **Merge** wird das `driver` Feld zum Array erweitert:

```yaml
driver:
  - "[[research/machine-customers-verified]]"
  - "[[research/ai-commerce-trends-verified]]"
```

---

## Edge Cases

### Forschung ohne klare Impacts

```
Claude: Analyse abgeschlossen.

        Ich konnte keine spezifischen Geschaeftsmodell-Impacts ableiten.

        Moegliche Gruende:
        - Report ist zu allgemein / zu wenig spezifisch
        - Befunde sind deskriptiv ohne normativen Handlungsdruck

        Empfehlung: Report manuell auf implizite Chancen pruefen,
        oder /knvs:ideate direkt mit dem Research als Quelle nutzen.
```

### Kein nutzbarer Report vorhanden

```
Claude: Keine Research Reports in research/ gefunden.

        Optionen:
        - Datei in research/ anlegen mit status: verified oder status: draft
        - Hypothesen (status: hypothesis) sind keine Research Reports und werden nicht gelistet
```

---

## Notes

- Impact Atoms sind KEINE Business Model Canvases
- Sie sind Pre-Ideate Dokumente die zwischen Research und BMC stehen
- Impacts sind nach Erstellung unveraenderlich (immutable)
- **Impact Atoms enthalten KEINE Severity-Bewertung.** Severity wird erst bei Canvas-Einbettung (`/knvs:ideate`) vergeben, weil sie den Kontext eines konkreten Geschaeftsmodells braucht.
- Loeschen/Archivieren ist manuell moeglich
- Wiki-Links nutzen `[[research/filename]]` Format (Obsidian-kompatibel)
- "Alle analysieren" ist Natural Language, kein CLI-Flag

---

## Impact Atom Template

Kanonisches Template fuer Impact Atoms:

```markdown
---
type: impact-atom
title: "[Kurzer beschreibender Titel]"
tags: [tag1, tag2]
driver: "[[research/filename]]"
bmc_fields:
  - Value Proposition
  - Key Resources
created: YYYY-MM-DD
---

# [IMPACT] Titel

**Das Szenario:**
[Was passiert / was veraendert sich? Konkret und faktenbasiert aus der Forschung.]

**Der Druck auf Geschaeftsmodelle:**
* **[BMC-Feld 1]:** [Konkreter Druck oder Chance fuer dieses Feld]
* **[BMC-Feld 2]:** [Konkreter Druck oder Chance fuer dieses Feld]

**Quelle:** [[research/filename]] - [Relevanter Befund/Abschnitt]
```

### Dateiname

Pattern: `impacts/<impact-slug>.md`

Beispiele:
- `impacts/vector-search-mandatory.md`
- `impacts/schema-org-pflicht.md`
- `impacts/algorithmic-pricing.md`

Die Research-Herkunft steht im `driver`-Frontmatter-Feld, nicht im Dateinamen.
