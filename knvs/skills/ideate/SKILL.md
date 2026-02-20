---
name: "ideate"
description: "Create a new Business Model Canvas in the IDEATE phase"
---

# /knvs:ideate

## Purpose

Creates a new Business Model Canvas in the `ideate/` folder.

## When to Use

- When you have a new business idea and want to capture it systematically
- When you have a research report and want to transform it into a Business Model Canvas
- Starting the innovation lifecycle for a new concept

## Workflow

### From Scratch

```
User: /knvs:ideate
Claude: What's your idea? Describe your initial thoughts...
User: [Describes idea]
Claude: Business Model Canvas created: "[Title]"
        File: ideate/[slug].md
        Status: IDEATE | Progress: WIP
```

### From Verified Research

```
User: /knvs:ideate
Claude: Do you want to base this on existing research? [Y/n]
User: Y
Claude: Available research:
        1. AI Bookkeeping Analysis [VERIFIED]
        2. Fitness Market Study [DRAFT]
        3. Vendor X KI-Wandel [HYPOTHESIS] (nicht verfuegbar)

        Which research report?
User: 1
Claude: Creating Business Model Canvas from research...

        Extracted from research:
        - Value Proposition: [from research findings]
        - Customer Segments: [from target audience]
        - Revenue Streams: [from market analysis]

        Business Model Canvas created: "AI Bookkeeping"
        File: ideate/ai-bookkeeping.md
        Status: IDEATE | Progress: WIP
        Source: research/ai-bookkeeping-analysis-verified.md
```

## What the Skill Does

1. Checks if research reports exist in `research/` folder (verified or draft; hypotheses excluded)
2. If yes: asks if starting from scratch or from research
3. If from research: lists verified and draft reports, pre-fills BMC sections from findings
4. If from research: scans `impacts/` for atoms where `driver` matches the selected research
5. If impacts found: displays them grouped by BMC field before canvas creation
6. If from scratch: asks for idea description
7. Automatically extracts appropriate title
8. Creates file in `ideate/` folder as `[slug].md` — slug is kebab-case derived from the title.
   NEVER add a date prefix or sequential number.
   CORRECT: `ai-bookkeeping-app.md`
   WRONG:   `20260220-001-ai-bookkeeping-app.md`
9. Initializes BMC template in IDEATE phase (Progress: WIP)
10. Adds initial thoughts or research findings as notes
11. If impacts exist: generates inline impact callouts under affected BMC dimensions

## Impact-Aware Canvas Creation

When creating a BMC from research, the skill automatically surfaces related Impact Atoms.

### Process

1. User selects a research report
2. Skill scans `impacts/` for atoms where `driver` contains the selected research
3. Groups impacts by `bmc_fields`
4. Displays impact summary, then generates canvas with inline callouts:

```
Claude: Erstelle BMC aus research/machine-customers-verified.md...

        3 Impact Atoms gefunden fuer diese Research:
        - vector-search-mandatory -> Value Prop, Key Resources
        - agent-negotiation -> Value Prop
        - algorithmic-pricing -> Revenue Streams

        Bewerte Severity fuer diesen Canvas...
        [Severity Assessment fuer jeden Impact, siehe oben]

        Generiere Canvas mit inline Impact-Callouts...

        Value Proposition: 2 Impacts eingebettet (1x HIGH, 1x MEDIUM)
        Key Resources: 1 Impact eingebettet (HIGH)
        Revenue Streams: 1 Impact eingebettet (MEDIUM)
        Andere Dimensionen: Keine Impacts

        Canvas erstellt: ideate/machine-customers.md
```

5. For each impact: assesses severity in context of THIS specific BMC (see Severity Assessment below)
6. Each affected BMC dimension gets inline `> [!impact]` callouts with field-specific reasoning and assessed severity

### Vendor-Risk Awareness

After canvas creation, the skill checks if any referenced vendor/partner in the canvas
has a corresponding Network Canvas in `network/`. If yes, a `[!vendor-risk]` callout
is generated under the relevant BMC field.

**Erkennung:** Wenn in Key Partnerships, Key Resources oder anderen Feldern eine
Organisation genannt wird die als `entity` in einem `network/*.md` vorkommt.

**Prozess:**
1. Scant alle BMC-Felder des neuen Canvas auf bekannte Vendor/Partner-Namen
2. Gleicht mit `network/*.md` Dateien ab (`entity`-Feld im Frontmatter)
3a. Wenn Treffer: `[!vendor-risk]`-Callout unter dem betroffenen Feld einfügen
3b. Wenn kein Treffer (Org bekannt, aber kein Network Canvas): Anlegen anbieten

**Callout-Format:**
```markdown
> [!vendor-risk]- [Entity]: [relationship] mit offenen Impacts ([[network/<slug>]])
> Aktuelle Situation: [optionale Kurzzusammenfassung des Vendor-Status]
```

**Beispiel - Network Canvas vorhanden:**
```
Claude: "Vendor Corp/PlatformX" in Key Partnerships gefunden.
        Network Canvas gefunden: network/vendor-corp.md (2 Impacts)

        -> [!vendor-risk]-Callout in Key Partnerships eingefügt:

        > [!vendor-risk]- Vendor Corp/PlatformX: Vendor unter Druck ([[network/vendor-corp]])
        > Aktuelle Situation: 2 Impacts auf Vendor-Modell (1 HIGH, 1 MEDIUM).
```

**Beispiel - Network Canvas fehlt:**
```
Claude: "Cloud Vendor" in Key Partnerships erwähnt.
        Kein Network Canvas für Cloud Vendor gefunden.

        Network Canvas anlegen?
        [J] Jetzt anlegen (leer, ohne Impacts)
        [N] Später - /knvs:impact auf Cloud-Vendor-Research anwenden
        [A] Ignorieren (kein Vendor-Tracking für Cloud Vendor)

User: J

Claude: Beziehung zu Cloud Vendor?
        [1] vendor [2] partner [3] customer [4] competitor [5] industry
User: 1

Claude: Network Canvas erstellt: network/cloud-vendor.md (leer)
        -> Tipp: /knvs:impact auf Research über Cloud Vendor anwenden um Impacts einzubetten.
        -> [!vendor-risk]-Callout in Key Partnerships eingefügt (keine Impacts bisher).
```

`[!vendor-risk]` ist KEIN Impact Atom. Es erscheint NICHT in `impacts/`.
Die eigentlichen Impacts leben im Network Canvas (`network/vendor-corp.md`).

### No Impacts Available

If no impacts exist for the selected research, the skill proceeds normally:

```
Claude: Keine Impact Atoms fuer diese Research gefunden.
        Tipp: /knvs:impact nutzen um Impacts vor der BMC-Erstellung zu extrahieren.
        Fortfahren ohne Impact-Kontext...
```

---

## Severity Assessment

Impact Atoms enthalten **keine Severity**. Severity wird erst hier vergeben - im Kontext des konkreten BMC.

### Warum hier und nicht im Impact?

Ein Impact beschreibt WAS passiert (Fakt). Wie schwer das wiegt, haengt vom konkreten Geschaeftsmodell ab:
- "Algorithmische Preisvergleiche" ist **High** fuer eine B2C-Preisvergleichs-Plattform (Kern-Value-Prop betroffen)
- Derselbe Impact ist **Low** fuer B2B Enterprise SaaS (feste Jahresvertraege, kein Spot-Pricing)

### Severity-Kriterien (BMC-kontextspezifisch)

| Severity | Kriterium |
|----------|-----------|
| **High** | Existenzbedrohend fuer DIESES Geschaeftsmodell. Kernelemente des Canvas direkt betroffen. |
| **Medium** | Wettbewerbsnachteil fuer DIESES Geschaeftsmodell. Anpassung noetig, aber kein Totalausfall. |
| **Low** | Optimierungspotenzial fuer DIESES Geschaeftsmodell. Nice-to-have, nicht geschaeftskritisch. |

### Prozess

1. Fuer jeden Impact Atom: Analysiere wie er die konkreten BMC-Dimensionen dieses Canvas betrifft
2. Beruecksichtige: Value Proposition, Customer Segments, Revenue Streams des Canvas
3. Schlage Severity vor mit Begruendung
4. User bestaetigt oder aendert

### Beispiel

```
Claude: 3 Impact Atoms gefunden fuer diese Research.
        Bewerte Severity fuer diesen Canvas (B2C Preisvergleichs-Plattform)...

        Impact 1: Algorithmische Preisvergleiche
        BMC-Felder: Revenue Streams, Channels
        Analyse: Transparenz ist Kern-Value-Prop. Algorithmische Kaeufer
                 umgehen die UI und damit das Provisionsmodell.
        Vorgeschlagene Severity: HIGH

        -> Uebernehmen? [J/n/bearbeiten]

User: J

Claude: Impact 2: Vektor-Suche wird Standard
        BMC-Felder: Key Resources
        Analyse: Plattform basiert auf strukturierten Produktdaten,
                 nicht auf Freitext-Suche. Vektor-Kompetenz ist nice-to-have.
        Vorgeschlagene Severity: LOW

        -> Uebernehmen? [J/n/bearbeiten]
```

### Severity-Dispute: Hypothesen-Erfassung

Wenn der User die Severity ablehnt und seine Begruendung auf einer
unverifizierten Marktannahme basiert:

1. Annahme identifizieren und als Einzeiler formulieren
2. Fragen: "Das ist keine gesicherte Erkenntnis aus der Research.
   Soll ich eine Hypothese als Recherche-Auftrag erfassen?"
3. Bei Ja: Datei in `research/<slug>.md` mit `status: hypothesis` erstellen
   (siehe Hypothese-Template unten)
4. Severity des Users im Canvas uebernehmen
5. Im Callout-Reasoning Backlink ergaenzen:
   `(Severity basiert auf Hypothese: [[research/<slug>]])`

Hypothesen-Erfassung NUR anbieten wenn die Begruendung eine unbestaetigte
Marktannahme enthaelt, nicht bei Meinungsverschiedenheit zur Analyse.

```
Claude: Impact 2: Vektor-Suche wird Standard
        Vorgeschlagene Severity: LOW
        -> Uebernehmen? [J/n/bearbeiten]

User:   n - HIGH, weil Vendor X den KI-Wandel verschlaeft

Claude: Diese Einstufung basiert auf einer unverifizierten Annahme:
        "Vendor X verschlaeft den KI-Wandel"

        Soll ich eine Hypothese als Recherche-Auftrag erfassen?
        [J] Hypothese erfassen + Severity uebernehmen
        [N] Severity ohne Erfassung uebernehmen
        [A] Bei LOW bleiben

User:   J

Claude: -> research/vendor-x-ki-wandel.md erstellt
        -> Severity HIGH uebernommen (mit Hypothese-Backlink im Callout)
```

---

## Hypothese-Template

Kanonisches Template fuer Hypothesen-Dateien in `research/`:

```markdown
---
type: research
title: "Vendor X verschlaeft den KI-Wandel"
status: hypothesis
created: YYYY-MM-DD
claim: "Vendor X verschlaeft den KI-Wandel und verliert dadurch Relevanz am Markt"
bmc_fields:
  - Key Partnerships
  - Value Proposition
origin_impact: impacts/vektor-suche-standard.md
---

# Hypothese: Vendor X verschlaeft den KI-Wandel

**Status:** HYPOTHESE (unbestaetigt - kein Research-Finding)

## Behauptung

Vendor X verschlaeft den KI-Wandel und verliert dadurch Relevanz am Markt.
Das haette direkte Auswirkungen auf Key Partnerships und Value Proposition,
weil [Kontext und Nuancen die ueber den claim hinausgehen].

## Kontext

**Entstanden bei:** /knvs:ideate Severity-Bewertung
**Canvas:** ideate/mein-canvas.md
**Impact:** impacts/vektor-suche-standard.md
**Severity-Entscheidung:** HIGH gesetzt (statt vorgeschlagenem LOW)

## Recherche-Auftrag

- [ ] Vendor X Produktstrategie und KI-Roadmap pruefen
- [ ] Branchenanalysten-Einschaetzungen einholen
- [ ] Wettbewerber-Positionierung vergleichen
```

---

## Inline Impact Callout Format

When creating a BMC from research with related impacts, embed callouts directly under affected dimensions.

### Generation Rules

1. **For each BMC dimension (`##` heading):**
   - Check if any impact atom has this dimension in `bmc_fields` array
   - If yes: generate inline callout immediately after the `##` heading, BEFORE the comment hint and content

2. **Callout structure:**
   ```markdown
   > [!impact]- [Impact Title from frontmatter] ([SEVERITY])
   > **Driver:** [[research/source-slug from driver field]]
   >
   > [Field-specific reasoning from "Der Druck" bullet for this dimension]
   >
   > _Quelle: [[impacts/impact-filename]]_
   ```

3. **Multiple impacts per dimension:**
   - Stack vertically (separate callouts, blank line between)
   - Order by severity: HIGH → MEDIUM → LOW

4. **Dimensions without impacts:**
   - Leave unchanged (only comment hint, no placeholder)

### Field-Specific Reasoning Extraction

Extract the relevant bullet from impact atom's "Der Druck auf Geschaeftsmodelle" section.

**Example impact atom body:**
```markdown
**Der Druck auf Geschaeftsmodelle:**
* **Value Proposition:** Systeme, die nur "Visuals" liefern, werden unsichtbar.
* **Key Resources:** Vektor-Datenbanken werden Pflicht.
```

**Generated under `## Value Proposition`:**
```markdown
> [!impact]- Semantische Vektoren statt Bilder (HIGH)
> **Driver:** [[research/machine-customers-verified]]
>
> Systeme, die nur "Visuals" liefern, werden unsichtbar.
>
> _Quelle: [[impacts/machine-customers--vector-search-mandatory]]_
```

**Generated under `## Key Resources`:**
```markdown
> [!impact]- Semantische Vektoren statt Bilder (HIGH)
> **Driver:** [[research/machine-customers-verified]]
>
> Vektor-Datenbanken werden Pflicht.
>
> _Quelle: [[impacts/machine-customers--vector-search-mandatory]]_
```

### Edge Cases

**Missing field-specific reasoning:**
If impact atom lists a BMC field in `bmc_fields` but has no matching bullet under "Der Druck":
Fallback to "Das Szenario" text from the impact atom.

**Multi-driver impacts:**
Show all drivers comma-separated:
```markdown
> [!impact]- Titel (HIGH)
> **Driver:** [[research/source1]], [[research/source2]]
```

---

## Progress States

IDEATE phase has two progress states:

| State | Description |
|-------|-------------|
| **WIP** | Work in progress, still researching/brainstorming |
| **READY FOR EXPLORE** | Research complete, ready to validate hypotheses |

> **STRICT:** Only these two values are valid for `progress`. Do NOT invent percentages or sub-states.
> CORRECT: `progress: WIP`  —  WRONG: `progress: WIP (30%)`

## Notes

- File is created in `ideate/` folder
- Filename: ONLY `[title-as-slug].md` — no date prefix, no sequential number, no number at all
- Initial progress is always WIP
- Use `/knvs:explore` when ready to move to validation phase

## Next Steps

After creating your canvas:
1. Open the file in your Markdown editor (Obsidian, VS Code, etc.)
2. Fill out all BMC sections (Value Proposition, Customer Segments, etc.)
3. Research and document your assumptions
4. When research is complete, set `progress: READY FOR EXPLORE`
5. Start validation with `/knvs:explore`

---

## Canvas Template

Canonical template for new IDEATE canvases.

> **STRICT:** Use ONLY the frontmatter fields listed below — do NOT add extra fields.
> Follow the structure exactly: do not rename sections, change heading levels, or add sections.

```markdown
---
status: IDEATE
progress: WIP
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_research: research/filename.md  # ONLY include when created from research. DELETE this line otherwise.
---

# Business Model Canvas: [Name]

## Value Proposition

<!-- Inline impact callouts appear here when created from research with impacts.
Example:

> [!impact]- Impact Title (HIGH)
> **Driver:** [[research/source-slug]]
>
> Field-specific reasoning explaining pressure on this BMC dimension.
>
> _Quelle: [[impacts/impact-slug]]_

Multiple impacts stack vertically, sorted by severity (HIGH > MEDIUM > LOW).
-->

<!-- What value do we deliver? What problem do we solve? -->

## Customer Segments
<!-- Who are we creating value for? -->

## Channels
<!-- How do we reach our customers? -->

## Customer Relationships
<!-- What type of relationship does each segment expect? -->

## Revenue Streams
<!-- What are customers willing to pay? -->

## Key Resources
<!-- What resources does our value proposition require? -->

## Key Activities
<!-- What activities does our value proposition require? -->

## Key Partnerships
<!-- Who are our key partners and suppliers? -->

## Cost Structure
<!-- What are the most important costs? -->

---

## Notes
<!-- Initial thoughts, research notes, open questions -->

## Next Steps
- [ ]
```

