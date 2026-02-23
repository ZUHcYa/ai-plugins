---
name: "produce"
description: "Production consultation - arrangement, harmony, sound design, rhythm, mixing, mastering guidance for songs in progress"
---

# /music-production:produce

## Purpose

Beratender Produktionsbegleiter fuer Songs in der Produktionsphase. Bietet Expertise in Arrangement, Harmonielehre, Sound Design, Rhythmus/Groove, Mixing und Mastering. Hilft dabei, einen Song fertigzustellen, indem konkrete Vorschlaege gemacht und Produktionsentscheidungen gemeinsam durchdacht werden.

## When to Use

- Song in `01_Projects/` haengt fest oder braucht Richtung
- Bestimmte Produktionsentscheidung ansteht (Arrangement, Mix, Mastering)
- Mix klingt nicht wie gewuenscht
- Arrangement-Ideen fehlen oder Struktur unklar
- Harmonie/Akkordfolge braucht Feedback
- Allgemeine Musikproduktionsfragen ohne konkreten Song

## Workflow

### Song-Auswahl und Context Loading

```
User: /music-production:produce
Claude: An welchem Song arbeitest du?

        01_Projects/
        1. 2026-02-Nightfall    [production] - 3 Tage
        2. 2026-01-Echoes       [production] - 28 Tage

        [S] Skip - kein bestimmter Song / allgemeine Frage

User: 1
Claude: Lese 2026-02-Nightfall.md...

        Nightfall
        Genre: (nicht gesetzt) | BPM: (nicht gesetzt) | Key: (nicht gesetzt)
        Referenzen: Radiohead - Everything In Its Right Place (schwebende Atmosphaere)
        Notizen: "dark atmospheric track, minimale Beats, schwebende Synth-Pads"
        TODOs: 3 offene Punkte
```

### Focus Selection

```
Claude: Woran moechtest du arbeiten?

        [A] Arrangement - Struktur, Energy Curve, Transitions
        [H] Harmony & Theory - Akkorde, Key, Modulation, Voicings
        [S] Sound Design - Synthese, Texturen, Klangpalette
        [R] Rhythm & Groove - Beats, Swing, Polyrhythm
        [M] Mixing - EQ, Kompression, Stereo, Tiefenstaffelung
        [X] Mastering - Lautheit, Limiting, Stereo Width

        Oder beschreib dein Problem in eigenen Worten.
```

### Focused Consultation (Beispiel: Arrangement)

```
User: Der Chorus klingt duenn im Vergleich zur Strophe

Claude: Das ist ein Energy-Curve-Problem. Die Strophe hat
        "schwebende Synth-Pads + minimale Beats". Wenn der Chorus
        auch Pad-basiert ist, fehlt der Kontrast.

        Typische Escalation-Tools:
        - Frequenz: Obere Harmonische hinzufuegen (Air, Brightness)
        - Dichte: Haupt-Element doppeln oder layern
        - Breite: Strophe eng, Chorus breit
        - Rhythmus: Rhythmische Aktivitaet ergaenzen (Hats, Arps, Sidechain)
        - Harmonie: Voicings oeffnen, Bass-Oktave verschieben

        Fuer einen "dark atmospheric" Track der dunkel bleiben soll:
        - Statt Lautstaerke-Eskalation: TEXTUR-Eskalation
        - Granulare oder verzerrte Layer unter den Pads
        - Sub-Bass Drop am Chorus-Anfang fuer physischen Impact

        Welche Richtung interessiert dich?

User: Textur-Eskalation klingt gut, und Sub-Bass

Claude: Gute Kombination. Konkrete Vorschlaege:

        TEXTUR-LAYER:
        1. Nimm dein Pad, dupliziere es, jage es durch einen
           Granular-Effekt (z.B. Clouds-Typ). Nur im Chorus aktiv.
        2. Alternativ: dezente Saturation/Distortion auf einem
           parallelen Bus - nur im Chorus einblenden.
        3. Beide Varianten low-cut bei ~200Hz damit sie dem
           Sub nicht in die Quere kommen.

        SUB-BASS:
        1. Reiner Sinus oder leicht gesaettigter Sub auf dem
           Grundton. Erst am Chorus-Downbeat einsetzen.
        2. Sidechain vom Kick, kurzer Release.
        3. Mono, centered - dafuer die Pads duerfen breiter
           werden im Chorus.

        Willst du noch an der Transition Strophe->Chorus arbeiten,
        oder passt die Richtung?
```

### Summary und Next Steps

```
Claude: Session-Zusammenfassung: Nightfall - Arrangement
        =============================================

        Problem: Chorus klingt duenn im Vergleich zur Strophe

        Erkenntnisse:
        - Strophe besetzt bereits den "schwebenden" Raum.
          Chorus braucht texturellen Kontrast, nicht Lautstaerke.
        - Referenz (Radiohead) erreicht Intensitaet durch
          Layering und Filterung, nicht durch strukturelle Spruenge.

        Action Items:
        1. Granularen oder verzerrten Textur-Layer unter Chorus-Pads
        2. Sub-Bass Einsatz am Chorus-Downbeat
        3. Stereofeld im Chorus verbreitern (Strophe enger)
        4. Sidechain auf Sub-Bass vom Kick

        In deine Song-TODOs uebernehmen? [J/n]

User: J
Claude: 4 Punkte zu 2026-02-Nightfall.md TODOs hinzugefuegt.
```

### Freie Beratung (ohne Song)

```
User: /music-production:produce
Claude: An welchem Song arbeitest du?

        01_Projects/
        1. 2026-02-Nightfall    [production] - 3 Tage

        [S] Skip - kein bestimmter Song / allgemeine Frage

User: S
Claude: Kein Problem. Was ist deine Frage?

User: Wie bekomme ich einen Track club-tauglich laut ohne
      dass er anfaengt zu pumpen?

Claude: Das ist eine Mastering-Frage mit Mixing-Implikationen.

        MIXING-SEITE (Vorbereitung):
        - Headroom: Mix sollte bei -6dB bis -3dB True Peak peaken
        - Low-End Kontrolle: Alles unter 30Hz hochpassen
        - Dynamik: Moderate Buskompression (2-4dB GR) vor dem Master
        - Transient Balance: Kicks nicht zu spitz, eher rund

        MASTERING-SEITE:
        - Ziel-Lautheit: -8 bis -6 LUFS fuer Club
        - Mehrstufiges Limiting statt einem harten Limiter:
          1. Clipper fuer Transienten (~2dB)
          2. Limiter fuer die restliche Reduktion (~3-4dB)
        - Das Pumpen kommt meist von zu viel Low-End
          im Limiter. Multiband-Limiting oder vorher ein
          dynamischer EQ auf 60-100Hz hilft.
        - A/B mit Referenz bei gleicher Lautheit (LUFS-Match)

        Soll ich tiefer in einen dieser Bereiche gehen?
```

## What the Skill Does

1. Pruefe ob `.music-production/config.json` existiert. Falls nicht: Hinweis auf `/music-production:start` und stoppen.
2. Scanne `01_Projects/` nach Song-Ordnern mit `status: production`.
3. Liste anzeigen. Nutzer waehlt Song ODER skippt (freie Beratung).
4. Falls Song gewaehlt: `<Ordnername>.md` komplett lesen (Frontmatter, Referenzen, Notizen, TODOs).
5. **Strategy-Kontext laden:** Falls `.music-production/strategy.md` existiert, lies es und wende an:
   - `## Sound`: Aesthetische Identitaet — Beratung im Stil dieser Identitaet geben, nicht generisch. Referenzen aus Strategy als stilistischen Anker nutzen.
   - `## Constraints`: Feste Produktionsregeln (z.B. Stereo-only, kein Multitrack) als harte Grenzen in Vorschlaegen beruecksichtigen. Nie etwas vorschlagen, das gegen Constraints verstoesst.
   - `## Hardware`: Vorschlaege auf vorhandenes Equipment beziehen. Statt "nimm einen Kompressor" sag "auf dem Tonverk koenntest du den Compressor-Effekt auf Track X nutzen".
   - `anti_genre` aus Frontmatter: Nie in Richtung der Anti-Genres beraten. Wenn eine Idee danach klingt, explizit darauf hinweisen.
   - Ohne Strategy: Skill funktioniert weiterhin generisch (wie bisher).
6. Focus-Area-Picker anzeigen (A/H/S/R/M/X) ODER freie Problembeschreibung akzeptieren.
7. Falls Freitext: relevante Focus-Areas aus der Beschreibung ableiten.
8. Focused Consultation fuehren. Guiding Questions als Geruest nutzen (nicht als starre Checkliste). An das anpassen, was der Nutzer tatsaechlich braucht.
9. Song-Kontext verwenden (Referenzen, Notizen, Genre, Key, BPM falls vorhanden) fuer spezifische statt generische Beratung.
10. Wenn Consultation einen natuerlichen Endpunkt erreicht oder Nutzer signalisiert: Zusammenfassung mit Key Insights und Action Items.
11. Falls Song gewaehlt: anbieten, Action Items in `## TODOs` zu schreiben.
12. Falls Song gewaehlt: anbieten, Consultation-Notizen in `## Notizen` anzuhaengen (optional).
13. Falls Nutzer BPM/Key im Dialog nennt und diese noch nicht im Frontmatter stehen: anbieten, sie einzutragen.

## Focus Areas

Jeder Bereich hat Guiding Questions, die als unsichtbares Geruest im Dialog dienen. Sie sind auch als eigenstaendige Checkliste nutzbar (Manual Workflow ohne Claude Code).

### Arrangement

| Dimension | Frage |
|-----------|-------|
| Struktur | Welche Sections hat der Song? Fehlt etwas (Bridge, Breakdown, Pre-Chorus)? |
| Energy Curve | Fuehlt sich jede Section wie ein klarer Schritt nach oben oder unten an? |
| Kontrast | Was aendert sich zwischen Sections? (Dichte, Frequenz, Rhythmus, Breite) |
| Transitions | Wie fliesst eine Section in die naechste? (Cut, Fade, Riser, Filter Sweep) |
| Intro/Outro | Etabliert das Intro die Welt? Loest das Outro auf oder fadet? |
| Laenge | Ist eine Section zu lang? Bleibt etwas zu lange? |
| Referenz-Fit | Passt das Arrangement zur Intention der Referenztracks? |

### Harmony & Theory

| Dimension | Frage |
|-----------|-------|
| Key/Mode | Welche Tonart? Dur, Moll, modal? Passt der Mode zur Stimmung? |
| Progression | Welche Akkorde? Diatonisch oder geliehen? Unerwartete harmonische Farbe? |
| Bass-Bewegung | Nur Grundtoene oder melodische Bewegung im Bass? |
| Voicings | Offen oder geschlossen? Wo im Frequenzspektrum sitzen die Akkorde? |
| Spannung/Aufloesung | Wo ist der harmonische Hoehepunkt? Aufgeloest oder offen gelassen? |
| Modulation | Aendert sich die Tonart? Wuerde Modulation der Energy Curve dienen? |

### Sound Design

| Dimension | Frage |
|-----------|-------|
| Palette | Was sind die 3-5 Kernsounds? Gehoeren sie in dieselbe Welt? |
| Textur | Genug Frequenzabdeckung? Low, Mid, High, Air? |
| Layering | Aehnliche Sounds gelayert fuer Dicke oder konkurrierend um Platz? |
| Bewegung | Entwickeln sich Sounds ueber Zeit? (LFO, Automation, Filter Sweeps) |
| Charakter | Was macht den Sound dieses Songs einzigartig? Was ist das Signature-Element? |
| Genre-Fit | Passen die Sounds zur Genre-Erwartung? Bewusste Abweichungen? |

### Rhythm & Groove

| Dimension | Frage |
|-----------|-------|
| Foundation | Wie ist das Kick/Snare-Pattern? Treibend oder schwebend? |
| Groove | Straight oder Swing? Wie viel Swing? |
| Layer | Hats, Percussion, Rides - wie viele rhythmische Ebenen? Zu wenige oder zu viele? |
| Raum | Wo sind die Pausen? Atmet der Beat? |
| Polyrhythmik | Cross-Rhythmen oder metrische Spannung? |
| Genre-Puls | Passt der Rhythmus zu Genre-Konventionen? |

### Mixing

| Dimension | Frage |
|-----------|-------|
| Balance | Ist jedes Element klar hoerbar? |
| Frequenz | Matsch in den Low-Mids (200-500Hz)? Haerte in den Hoehen (2-5kHz)? |
| Stereo | Ist die Mitte stark (Kick, Bass, Lead)? Sind Pads und Effekte breit? |
| Tiefe | Sitzen Elemente auf verschiedenen Entfernungen? (Reverb, Delay, Pegel) |
| Dynamik | Dient die Kompression dem Groove oder toetet sie ihn? |
| Bus-Processing | Sind Gruppen (Drums, Synths, Vocals) zusammengeklebt? |
| Referenz-Check | Haelt der Mix gegen den Referenztrack stand? |

### Mastering

| Dimension | Frage |
|-----------|-------|
| Lautheit | Welches LUFS-Ziel? (-14 Spotify, -16 Apple, -9 bis -6 Club) |
| Headroom | Peakt der Mix ueber -3dB True Peak vor dem Mastering? |
| Frequenz-Tilt | Fuehlt sich das Gesamtspektrum auf mehreren Systemen ausgewogen an? |
| Stereo Width | Ist der Master mono-kompatibel? |
| Limiting | Arbeitet der Limiter hart? Hoert man Artefakte? |
| Format | Welches Lieferformat? WAV 44.1/16? 48/24? Verschiedene Master fuer Plattformen? |

## Edge Cases

**Keine Songs in `01_Projects/`:**
```
Claude: Keine Songs in 01_Projects/ gefunden.

        [N] Neuen Song anlegen (/music-production:new)
        [F] Freie Beratung ohne bestimmten Song
```

**Song-Datei fast leer (frisches Template):**
Skill funktioniert auch mit minimalem Kontext. Mehr Kontext = spezifischere Beratung. Bei leerem File einfach fragen, wo der Nutzer in der Produktion steht.

**Nutzer will mehrere Focus Areas:**
Natuerlich behandeln. Zusammenhaengende Areas (z.B. Arrangement + Mixing) nacheinander oder verwoben besprechen, je nach Problem.

**BPM/Key werden im Dialog erwaehnt:**
```
User: Ist in Dm bei 128 BPM

Claude: Notiert. Soll ich bpm: 128 und key: Dm ins Frontmatter eintragen? [J/n]
```

## Notes

- Dieser Skill ist beratend. Alle Vorschlaege sind Angebote, keine Regeln. Musik ist subjektiv.
- Der Skill aendert Dateien NUR mit expliziter Nutzer-Bestaetigung.
- Aenderungen beschraenken sich auf: `## TODOs` anhaengen, `## Notizen` anhaengen, optional `bpm`/`key` im Frontmatter.
- Die Guiding-Questions-Tabellen funktionieren als eigenstaendige Checkliste fuer Nutzer ohne Claude Code.
- Genre-spezifische Beratung wird gegeben wenn Genre bekannt ist (aus Frontmatter oder Gespraech). Ohne Genre-Kontext bleibt Beratung allgemein.
- Referenztracks werden als stilistische Anker verwendet, nicht als Vorschrift.
- Der Skill funktioniert fuer jedes Genre. Er setzt nicht elektronisch, Pop oder einen bestimmten Stil voraus.

## Manual Workflow (ohne Claude Code)

Diesen Skill kann man auch komplett manuell nutzen:

1. Focus Area waehlen (Arrangement, Harmony, Sound Design, Rhythm, Mixing, Mastering)
2. Konkretes Problem identifizieren
3. Guiding Questions der gewaehlten Area durchgehen
4. Beobachtungen und naechste Schritte in `## Notizen` und `## TODOs` notieren
