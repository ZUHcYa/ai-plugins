---
name: "strategy"
description: "Define your artist identity, distribution strategy, and production philosophy - persistent context for all other skills"
---

# /music-production:strategy

## Purpose

Gefuehrter Dialog, der deine kuenstlerische Identitaet, Distribution-Strategie und Produktionsphilosophie in einem persistenten Dokument erfasst. Alle anderen Skills (`produce`, `generate`, `release`) lesen dieses Dokument fuer kontextbewusste statt generische Beratung.

## When to Use

- Erstmalige Einrichtung: kuenstlerische Identitaet definieren
- Identitaet hat sich weiterentwickelt: Sound, Plattformen, Gear veraendert
- `/music-production:start` schlaegt es vor (wenn noch keine Strategy existiert)

---

## Workflow

### Ersteinrichtung (keine strategy.md vorhanden)

```
User: /music-production:strategy
Claude: Willkommen zum Strategy-Setup!

        Hier definierst du deine kuenstlerische Identitaet, deine
        Distribution-Strategie und deine Produktionsregeln.
        Alle anderen Skills nutzen das fuer bessere, personalisierte Beratung.

        Wir gehen 5 Bloecke durch. Die ersten drei sind Pflicht,
        die letzten zwei kannst du jetzt oder spaeter ausfuellen.

        Los geht's mit deiner Artist Identity.

        Wie heisst du als Kuenstler / was ist dein Alias?
```

### Bereits vorhanden

```
User: /music-production:strategy
Claude: Strategy existiert bereits (.music-production/strategy.md)

        Artist: The Hardware Sculptor
        Sound: Organic Downtempo, Dark Ambient, DnB, UK Garage, Outsider House
        Anti-Genre: Techno
        Plattformen: YouTube, Bandcamp
        Hardware: 4 Geraete konfiguriert
        Zuletzt aktualisiert: 2026-02-21

        Was moechtest du tun?
        [1] Einzelne Sektion ueberarbeiten
        [2] Alles neu aufsetzen (ueberschreibt komplett)
        [3] Abbrechen
```

---

## Dialog-Struktur

### Block 1 — Artist Identity (Pflicht)

Fragen:

1. **Kuenstlername / Alias**
   - "Wie heisst du als Kuenstler / was ist dein Alias?"

2. **Sound-Beschreibung**
   - "Beschreib deinen Sound in 2-3 Saetzen. Was macht deinen Sound aus? Was ist das Gefuehl, das du erzeugst?"

3. **Referenzen**
   - "Nenne 3 Artists oder Tracks, die deinen Stil am besten definieren."

4. **Anti-Genre / Anti-Stil**
   - "Was machst du explizit NICHT? Welche Genres, Sounds oder Aesthetiken willst du bewusst vermeiden?"

### Block 2 — Distribution (Pflicht)

Fragen:

1. **Plattformen**
   - "Wo veroeffentlichst du deine Musik? (z.B. YouTube, Bandcamp, SoundCloud)"

2. **Ausgeschlossene Plattformen**
   - "Gibt es Plattformen, die du bewusst NICHT nutzt? (z.B. Spotify, DistroKid)"

3. **Sprache**
   - "In welcher Sprache sind deine oeffentlichen Inhalte? (Titel, Beschreibungen, Lyrics)"

### Block 3 — Constraints / Philosophie (Pflicht)

Fragen:

1. **Feste Regeln**
   - "Welche Regeln gelten fuer dich IMMER? (z.B. nur Stereo-Recording, kein Multitrack, immer dawless, bestimmte Zeitlimits)"

2. **Philosophie**
   - "Was ist deine Produktionsphilosophie in einem Satz? (z.B. 'Flow > Perfektion', 'First take or nothing', '80/20 — wenn es laenger als 15 Min dauert, ist der Ansatz falsch')"

### Block 4 — Hardware & Visual (Optional)

```
Claude: Die naechsten zwei Bloecke sind optional.
        Du kannst sie jetzt ausfuellen oder spaeter mit
        /music-production:strategy nachliefern.

        Hardware & Visual Identity einrichten? [J/n/spaeter]
```

Falls Ja:

1. **Hardware-Liste**
   - "Welches Equipment nutzt du? Liste die Geraete mit ihrer Rolle auf. (z.B. 'Elektron Tonverk = Primary Sampler, Mixing-Hub')"

2. **Visuelle Aesthetik**
   - "Beschreib die visuelle Richtung fuer deine Cover Art in 2-3 Saetzen. (z.B. 'Roh, organisch, Glitch-Elemente. Wabi-Sabi Aesthetik — Imperfection ist gewollt.')"

3. **Cover-Art Keywords**
   - "Welche Keywords sollen in AI-generierten Cover-Art-Prompts immer einfliessen? (z.B. 'collage style, torn paper effect, analog glitch, datamoshing')"

### Block 5 — Release-Zyklus (Optional)

```
Claude: Release-Zyklus dokumentieren? [J/n/spaeter]
```

Falls Ja:

1. **Rhythmus**
   - "Wie oft veroeffentlichst du? (z.B. monatlich, alle 6 Wochen, unregelmaessig)"

2. **Meilensteine**
   - "Hast du feste Meilensteine in deinem Release-Zyklus? (z.B. 'Taping Thursday = Audio-Deadline, Admin Monday = Packaging & Upload, Release Friday = Veroeffentlichung')"

---

## Was der Skill tut

1. Pruefe ob `.music-production/config.json` existiert. Falls nicht: Hinweis auf `/music-production:start` und stoppen.
2. Pruefe ob `.music-production/strategy.md` existiert.
3. **Falls vorhanden:**
   a. Frontmatter und Sektionen lesen
   b. Zusammenfassung anzeigen (Artist, Sound, Plattformen, Hardware-Count, letztes Update)
   c. Optionen anbieten: einzelne Sektion ueberarbeiten / alles neu / abbrechen
   d. Bei Einzelsektion: nur den relevanten Block-Dialog fuehren, Rest beibehalten
   e. Bei alles neu: Nutzer muss explizit bestaetigen (destruktiv)
4. **Falls nicht vorhanden:**
   a. Block 1-3 interaktiv durchgehen (Pflicht)
   b. Block 4-5 anbieten (optional, "spaeter" moeglich)
   c. Aus Antworten `strategy.md` generieren (Frontmatter + Sektionen)
   d. Datei nach `.music-production/strategy.md` schreiben
5. Bestaetigung anzeigen mit Zusammenfassung des Erstellten
6. Hinweis: "Deine anderen Skills (produce, generate) nutzen diese Strategy jetzt automatisch fuer personalisierte Beratung."

---

## strategy.md Template

```markdown
---
artist_name: "The Hardware Sculptor"
genre:
  - "Organic Downtempo"
  - "Dark Ambient"
  - "DnB"
  - "UK Garage"
  - "Outsider House"
anti_genre:
  - "Techno"
references:
  - "Bonobo - Black Sands"
  - "Burial - Untrue"
  - "Boards of Canada - Music Has the Right to Children"
platforms:
  - "YouTube"
  - "Bandcamp"
forbidden_platforms:
  - "Spotify"
  - "DistroKid"
language_public: "EN"
language_internal: "DE"
created: "2026-02-21"
updated: "2026-02-21"
---

# Studio Identity

## Sound

Organic Downtempo und Dark Ambient mit Einflüssen aus DnB, UK Garage und
Outsider House. Emotionaler, texturreicher Sound mit Fokus auf Atmosphäre
und Groove. Flow und Imperfection über Perfektion.

Nicht: Techno (kein gerader 4/4 Rumble).

## Constraints

- Stereo Only: Kein Multitrack-Recording. Mix passiert im Sampler.
- 80/20: Wenn ein Ansatz länger als 15 Min dauert, ist er falsch.
- Audio First: Video und Visuals sind sekundär, nie Selbstzweck.

## Hardware

| Gerät | Rolle |
|-------|-------|
| Elektron Tonverk | Primary Sampler, Mixing-Hub |
| Arturia Keystep MK2 | Sequencer |
| Soundcraft Notepad FX 12 | Recorder (Stereo) |
| Bitwig Studio | Mastering Only |

## Visual Identity

Wabi-Sabi Ästhetik: Roh, organisch, bewusst imperfekt. Glitch-Elemente,
analoge Texturen, keine cleanen Renderings.

**Cover-Art Keywords:** collage style, torn paper effect, analog glitch,
datamoshing, split composition, film grain

## Distribution

### Metadata-Syntax

- **YouTube Title:** `{Sound Hook} on {Hardware} | {Genre} | {BPM} bpm`
- **Bandcamp Title:** `{Abstrakter Ein-Wort-Name}` (z.B. "Oxidation", "Void", "Static")

### Beschreibungs-Template

1. "{Adjektiv} journey in {Key}. Recorded dawless on {Hardware}."
2. // SIGNAL CHAIN: + Performance: {Hardware} + Rec: Soundcraft + Master: Bitwig
3. // CONNECT: [Artist-Link]

### Tags

**Erlaubt:** outsiderhouse, idm, breakbeat, dawless, ambient, experimental, uk-garage
**Verboten:** techno

## Release-Zyklus

**Rhythmus:** Monatlich

**Meilensteine:**
- **Creation Phase (Wochen 1-3):** Raw Material — Audio + Video Aufnahmen
- **Taping Thursday (letzter Do):** Audio-Deadline. Commit to Sound. Keine Änderungen mehr.
- **Admin Monday (letzter Mo):** Packaging — Metadata, Texte, Artwork, Upload.
- **Release Friday (1. Fr):** Veröffentlichung.
```

---

## Hinweise zum Dialog

- Jede Frage einzeln stellen, nicht alle auf einmal. Natuerlich auf die Antwort eingehen.
- Bei Block 4 und 5: Wenn der Nutzer "spaeter" waehlt, die Sektionen NICHT als leere Platzhalter anlegen. Sie werden erst beim naechsten `/music-production:strategy`-Aufruf ergaenzt.
- Bei "Einzelne Sektion ueberarbeiten": Nur die Fragen des relevanten Blocks stellen. Den Rest der Datei unveraendert lassen.
- Frontmatter-Felder muessen maschinenlesbar bleiben (Listen als YAML-Arrays, keine Freitext-Listen).
- Der Nutzer darf `strategy.md` jederzeit manuell in einem Editor bearbeiten. Die Datei ist self-documenting.
- Das Template oben ist ein BEISPIEL. Die tatsaechlichen Inhalte kommen aus dem Dialog.

## Manual Workflow (ohne Claude Code)

Diesen Skill kann man auch komplett manuell nutzen:

1. `.music-production/strategy.md` im Editor erstellen
2. Frontmatter mit den Feldern aus dem Template ausfuellen
3. Sektionen (Sound, Constraints, Hardware, Visual Identity, Distribution, Release-Zyklus) als Freitext schreiben
4. Datei speichern — fertig. Andere Skills lesen sie automatisch.

## Notes

- `strategy.md` lebt in `.music-production/` (gitignored) — nicht im Plugin-Verzeichnis
- Die Datei ist persoenlich und projektspezifisch, nicht Teil des Plugins
- Frontmatter ist maschinenlesbar (wird von anderen Skills und Hooks geparsed)
- Sektionen sind Freitext fuer den Menschen (und fuer Claude als Kontext)
- Keine Sektion ist Voraussetzung fuer andere Skills — Strategy ist ein Enhancer, kein Blocker
