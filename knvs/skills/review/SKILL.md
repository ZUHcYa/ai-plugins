---
name: "review"
description: "Conduct disruption review for EXPLOIT phase canvases"
---

# /knvs:review

## Purpose

Reviews an EXPLOIT canvas for risks, opportunities, and disruption threats. Creates separate review files for history tracking.

## When to Use

- Quarterly reviews of running businesses
- When market conditions have changed
- Identifying disruption risks early

## Workflow

```
User: /knvs:review
Claude: Which business do you want to review?

        1. Invoice Tool (exploit/)
        2. AI Bookkeeping (exploit/)

User: 1
Claude: Starting Disruption Review...

        I'll analyze your business model across 5 areas:

        ## 1. Market & Competition
        - Current competitive landscape
        - New entrants / substitutes
        - Market share trends

        [Claude provides analysis and questions]

        ## 2. Technology
        ...

        ## 3. Regulation
        ...

        ## 4. Value Proposition
        ...

        ## 5. Revenue Streams
        ...

        ## External Factors
        I've identified these external factors:
        - EU AI Regulation 2027: Potential opportunity
        - Market consolidation: Medium risk

        Are there additional external factors to consider?

User: Yes, competitor just raised 5M

Claude: Noted. Now let's assess business model impact...

        ## Business Model Impact
        Based on findings, I recommend:
        - Value Proposition: Add DATEV integration
        - Revenue Streams: Improve premium tier

        Review saved: reviews/ai-bookkeeping/2026-01-26.md
        Canvas updated: exploit/ai-bookkeeping.md
        Next review: 2026-04-26
```

## Past Reviews

The skill can also list or display past reviews when the user asks
(e.g., "show past reviews for AI Bookkeeping" or "show the review from June 2026"):

```
User: /knvs:review  (then asks to see past reviews)
Claude: Reviews for AI Bookkeeping:

        | Date | Notes |
        |------|-------|
        | 2026-06-15 | Quarterly review |
        | 2026-03-15 | Initial review |

        Which review do you want to see?
```

## Impact Challenge Mode

After selecting a canvas for review, the skill offers impact-based analysis as **Interview-Dialog**.
Ziel ist Bewusstseinsbildung - nicht Bewertung ob der Canvas Impacts bereits abdeckt.
Anpassungen am Canvas erfolgen separat via `/knvs:ideate` (ggf. als geforkter Canvas).

```
Claude: Moechtest du den Canvas auch gegen Impact Atoms challengen?

        Optionen:
        [A] ALLE Impacts - Vollstaendiger Abgleich gegen alle Atoms in impacts/
        [N] NUR NEUE Impacts - Nur Atoms erstellt seit dem letzten Review (2026-01-26)
        [S] Skip - Normaler Review ohne Impact-Challenge

User: N
Claude: 2 Impacts gefunden. Gehen wir sie einzeln durch.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 1 von 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Titel: Semantische Vektoren statt Bilder
        Severity im Canvas: HIGH
        BMC-Felder: Value Proposition, Key Resources

        Das Szenario:
        Kuenftige Kaeufer (KI-Agenten) ignorieren reine HTML-Daten.
        Sie benoetigen Vektoren und strukturierte Attribute.

        Der Druck auf diesen Canvas:
        * Value Prop: Systeme, die nur "Visuals" liefern, werden unsichtbar.
        * Key Resources: Vektor-Datenbanken werden Pflicht.

        Wie schaetzt du diesen Impact aktuell ein?

User:   Relevant, aber wir haben schon angefangen umzubauen.

Claude: Gut. Heisst das, Key Resources ist schon angepasst,
        oder steht das noch aus?

User:   Steht noch aus, ist aber auf der Roadmap.

Claude: Verstanden. Weiter zum naechsten Impact.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        IMPACT 2 von 2
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        Titel: Algorithmische Preisvergleiche
        Severity im Canvas: – (noch nicht bewertet, kein Callout)
        BMC-Felder: Revenue Streams

        Das Szenario:
        Preisvergleich wird algorithmisch statt manuell.

        Der Druck auf diesen Canvas:
        * Revenue Streams: Provisionsmodelle werden durch direkte
          API-Verhandlungen umgangen.

        Dieser Impact ist noch nicht im Canvas eingebettet.
        Wenn relevant: Via /knvs:ideate einbetten und Severity
        im BMC-Kontext bewerten.

        Wie schaetzt du diesen Impact ein?

User:   Nicht relevant fuer unser Modell.

Claude: Notiert.

        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        ZUSAMMENFASSUNG
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        2 Impacts besprochen.
        Weiter mit dem regulaeren Disruption Review...
```

### How "New Since Last Review" Works

The skill determines "new" impacts by comparing:
- `created` date in each impact atom's frontmatter
- `review_date` of the most recent review file in `reviews/<canvas-name>/`
- If no previous review exists, ALL impacts are considered "new"

### Quellen fuer den Impact-Dialog

Der Interview-Dialog nutzt **zwei Quellen**:

**1. Canvas Inline-Callouts (Severity)**
- Enthaelt die kontextspezifische Severity fuer DIESEN Canvas
- Severity wurde bei Canvas-Erstellung (`/knvs:ideate`) im BMC-Kontext vergeben
- Wird im Dialog angezeigt (wenn vorhanden)

**2. Impact Atoms in impacts/ (Vollstaendigkeit)**
- Listet ALLE verfuegbaren Impacts fuer diese Research
- Erkennt Impacts die noch nicht im Canvas eingebettet sind (kein Callout)
- Impact Atoms enthalten KEINE Severity (nur Fakten)

**Dialog-Logik:**
1. Scanne `impacts/` nach allen Atoms die diesen Canvas betreffen koennten (via `driver` + `bmc_fields`)
2. Fuer JEDEN Impact: Pruefe ob Canvas-Callout existiert
3. Severity aus Canvas-Callout lesen (wenn vorhanden)
4. Bei fehlendem Callout: Im Dialog erwaehnen, Einbettung via `/knvs:ideate` empfehlen
5. User-Einschaetzung im Dialog besprechen, keine automatische Bewertung

**Wichtig:** Der Review veraendert den Canvas NICHT. Anpassungen erfolgen separat via `/knvs:ideate`.

Falls waehrend des Dialogs unbestaetigte Marktannahmen entstehen
(z.B. "Wettbewerber X wird den Markt verlassen"), siehe
"Hypothese (Research-Vorlaeuferstufe)" in CLAUDE.md.

---

## What the Skill Does

1. Lists all canvases in `exploit/` folder
2. User selects which business to review
3. Claude analyzes and provides insights
4. Reviews 5 key areas:
   - Market & Competition
   - Technology trends
   - Regulation & Compliance
   - Value Proposition relevance
   - Revenue Streams stability
5. Identifies External Factors (market, tech, regulation, economy)
6. Assesses Business Model Impact
7. Generates risk matrix (probability x impact)
8. Creates action items
9. Saves review to `reviews/<canvas-name>/YYYY-MM-DD.md`
10. Updates canvas with review reference
11. Sets next review date (default: quarterly)

## Review Areas

| Area | Key Questions |
|------|---------------|
| Market | New competitors? Market shifts? |
| Technology | Disruptive tech? Build vs buy? |
| Regulation | New laws? Compliance gaps? |
| Value Prop | Still relevant? Customer needs changed? |
| Revenue | Pricing pressure? New models needed? |

## File Structure

Reviews are stored in separate files for history tracking:

```
<targetPath>/
|- exploit/
|   |- ai-bookkeeping.md         # Canvas (references reviews)
|- reviews/
    |- ai-bookkeeping/           # Folder per canvas
        |- 2026-03-15.md         # Past review
        |- 2026-06-15.md         # Latest review
```

## Backwards Compatibility

- Existing canvases with inline `## Review History` continue to work
- Skill can read both formats (inline + separate files)
- New reviews always go to `reviews/` folder

## Notes

- Review frequency is typically quarterly
- External Factors track macro-level influences

---

## Canvas Template

Canonical template for disruption reviews:

```markdown
---
canvas: [canvas-name]
review_date: YYYY-MM-DD
participants: []
next_review: YYYY-MM-DD
---

# Disruption Review: [Canvas Name]

**Canvas:** [Link to exploit/canvas.md]
**Date:** YYYY-MM-DD
**Participants:** [Names or "Solo analysis"]

---

## Findings

| Area | Status | Notes |
|------|--------|-------|
| Market & Competition | | |
| Technology | | |
| Regulation | | |
| Value Proposition | | |
| Revenue Streams | | |

---

## Risk Matrix

| Risk | Probability | Impact | Priority | Owner |
|------|-------------|--------|----------|-------|
| | Low/Med/High | Low/Med/High | | |

---

## Open Items

| Item | Raised By | Category | Status |
|------|-----------|----------|--------|
| | | Question/Concern/Idea | Open/Resolved |

---

## External Factors

| Factor | Type | Impact | Timeframe |
|--------|------|--------|-----------|
| | Market/Tech/Regulation/Economy | Positive/Negative/Uncertain | Near/Mid/Long-term |

---

## Business Model Impact

| BMC Area | Current State | Recommended Change | Priority |
|----------|---------------|-------------------|----------|
| | | | |

---

## Action Items

- [ ] @Owner: Task (Due: YYYY-MM-DD)

---

## Summary

[Brief narrative summary]

---

**Next Review:** YYYY-MM-DD
```

