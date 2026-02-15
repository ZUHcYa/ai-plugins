---
name: "generate"
description: "Generate release texts (YouTube, Bandcamp, Cover Art) from song description"
---

# /music-production:generate

## Purpose

The core value skill. Reads the enriched song file (`<Ordnername>.md`) and generates all release texts so the user only needs to copy & paste.

## When to Use

- After `/music-production:release` has enriched the song file
- Song description, BPM, Key, Mood are filled in
- Ready to generate distribution texts

## Workflow

```
User: /music-production:generate
Claude: Which song needs release texts?

        02_Vault/
        1. 2026-02-Nightfall    [release] FW004

User: 1
Claude: Reading 2026-02-Nightfall.md...

        Song: Nightfall
        BPM: 92 | Key: Dm | Mood: melancholic
        Description: "Ein dunkler, atmosphaerischer Track..."

        Generating release texts...

        === YouTube ===
        Title: Nightfall
        Description:
        Ein dunkler, atmosphaerischer Track mit schwebenden Synth-Pads
        und minimalistischen Beats. Inspiriert von der Stille kurz vor
        Sonnenuntergang.

        BPM: 92 | Key: Dm

        Released under [Artist Name]
        ---
        [Bandcamp/Streaming links]

        === Bandcamp ===
        Title: Nightfall
        Description:
        Nightfall captures the silence just before sunset - dark,
        atmospheric synth pads over minimalist beats. Melancholic
        but never sad.

        Tags: electronic, ambient, dark, atmospheric, synth,
              melancholic, minimal

        === Cover Art Prompt ===
        A dark atmospheric landscape at dusk, moments before sunset.
        Minimal composition, deep shadows, muted warm tones fading
        into cool darkness. Abstract, no text. Mood: melancholic,
        contemplative. Style: digital art, minimal.

        All texts written to 2026-02-Nightfall.md.
        Review and adjust, then copy & paste to platforms.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists
2. List songs in `02_Vault/` with `status: release`
3. User selects song
4. Read `<Ordnername>.md` completely:
   - Frontmatter: BPM, Key, Mood, catalog_nr
   - Song-Beschreibung section
   - Referenzen section (for context)
   - Notizen section (for context)
5. Generate release texts:
   - **YouTube Title + Description**
   - **Bandcamp Title + Description + Tags**
   - **Cover Art Prompt**
6. Write generated texts into the existing `<Ordnername>.md` Release-Texte and Cover Art sections
7. Update `updated:` date in frontmatter

## Generation Guidelines

### YouTube Description
- Start with the song description (German or English, matching the original)
- Include BPM and Key as technical info
- Keep it concise (3-5 short paragraphs max)
- Leave placeholder for links and artist info

### Bandcamp Description
- Translate/adapt the description to English (Bandcamp audience is international)
- More poetic/evocative tone than YouTube
- Shorter than YouTube description

### Bandcamp Tags
- 5-10 relevant tags
- Mix of genre, mood, and descriptive tags
- Lowercase, comma-separated

### Cover Art Prompt
- Derive visual mood from song description
- Abstract/atmospheric preferred (no literal illustrations)
- Include style direction (digital art, photography, painting, etc.)
- Specify: no text on the image
- Usable with AI image generators (Midjourney, DALL-E, etc.)

---

## Notes

- Only works on songs with `status: release` (run `/music-production:release` first)
- All generated texts are written INTO the song file (`<Ordnername>.md`) - not to separate files
- User should review and adjust before publishing
- Can be re-run to regenerate texts (overwrites previous generation)
- The quality depends on the Song-Beschreibung - a richer description produces better results
