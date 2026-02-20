---
name: "generate"
description: "Generate release texts (YouTube, Bandcamp, Cover Art) from song description"
---

# /music-production:generate

## Purpose

The core value skill. Reads the enriched song file (`<FolderName>.md`) and generates all release texts so the user only needs to review and copy-paste to distribution platforms.

## When to Use

- After `/music-production:release` has enriched the song file with metadata and description
- BPM, Key, Genre, Mood, and Song-Beschreibung are filled in
- Ready to generate YouTube description, Bandcamp description, and cover art prompt

## Workflow

```
User: /music-production:generate
Claude: Which song needs release texts?

        02_Vault/ (status: release)
        1. 2026-02-Nightfall    [release] FW004

User: 1
Claude: Reading 2026-02-Nightfall.md...

        Song: Nightfall
        BPM: 92 | Key: Dm | Genre: electronic | Mood: melancholic
        Description: "A dark, atmospheric track with floating synth pads
        and minimalist beats. Inspired by the silence just before sunset.
        Melancholic but not sad."
        References: Radiohead - Everything In Its Right Place

        Generating release texts...

        [texts generated - see example output below]

        All texts written to 2026-02-Nightfall.md.
        Review and adjust, then copy & paste to platforms.
```

## What the Skill Does

1. Check that `.music-production/config.json` exists. If not: prompt user to run `/music-production:start` first and stop.
2. List songs in `02_Vault/` with `status: release`.
3. User selects song.
4. Read `<FolderName>.md` completely:
   - Frontmatter: BPM, Key, Genre, Mood, catalog_nr, title
   - `## Song-Beschreibung` section (primary input for generation)
   - `## Referenzen` section (used as stylistic context)
   - `## Notizen` section (used as additional context)
5. Generate all three release texts from the collected data:
   - YouTube title and description
   - Bandcamp title, description, and tags
   - Cover art prompt
6. Write the generated texts into the existing `## Release-Texte` and `## Cover Art` sections of `<FolderName>.md`.
7. Update `updated:` date in frontmatter.

---

## What the Skill Reads

| Source | Field | Used For |
|--------|-------|---------|
| Frontmatter | `bpm` | YouTube description (technical info block) |
| Frontmatter | `key` | YouTube description (technical info block) |
| Frontmatter | `genre` | Bandcamp tags, tone of description |
| Frontmatter | `mood` | Cover art prompt, Bandcamp tags |
| Frontmatter | `catalog_nr` | YouTube description footer |
| Derived from folder name | Song title | All texts |
| `## Song-Beschreibung` | Free-text description | Primary input for all texts |
| `## Referenzen` | Artist references | Stylistic context, not copied verbatim |
| `## Notizen` | Production notes | Additional context |

---

## Generation Guidelines

### What Makes a Good Release Text

Release texts serve two purposes simultaneously: they must catch the listener's attention (human readers browsing platforms) and satisfy platform search algorithms (tags, keywords). The description should:

- Open with the most evocative sentence first (hook)
- Convey mood and atmosphere before listing technical details
- Use concrete, sensory language rather than vague adjectives ("silence just before sunset" beats "peaceful track")
- Keep technical details (BPM, Key) factual and brief — they are for producers, not casual listeners
- End with a minimal call-to-action (follow link, label name)

### YouTube Description

**Structure:**
1. Hook / mood sentence (1-2 sentences — the most important part)
2. Expanded description (1-3 sentences — atmosphere, story, influences if notable)
3. Technical block (BPM, Key — brief, on separate lines)
4. Catalog line and artist/label name
5. Placeholder for links

**Rules:**
- Match the language of the Song-Beschreibung (German description -> German YouTube text; English description -> English text)
- 100-250 words total
- No hashtags in the main body (YouTube indexes them separately)
- Leave placeholder lines for Bandcamp/streaming links

### Bandcamp Description

**Structure:**
1. Adapted hook (may differ from YouTube — Bandcamp readers are deeper listeners)
2. Core description (atmosphere, sound design, concept — 2-4 sentences)
3. No technical block (BPM/Key are not typical Bandcamp content)

**Rules:**
- Always in English (Bandcamp's primary audience is international)
- If Song-Beschreibung is in German: translate and adapt (not just translate literally)
- More poetic/evocative tone than YouTube
- 50-150 words — quality over quantity
- Do not include links or catalog numbers in the body

### Bandcamp Tags

**Rules:**
- 5-10 tags total
- Include: genre (e.g. `electronic`), mood (e.g. `melancholic`), descriptive terms (e.g. `atmospheric`, `synth`, `minimal`)
- All lowercase
- No spaces within a tag — use hyphens if needed (e.g. `dark-ambient`)
- Comma-separated on a single line

### Cover Art Prompt

**Rules:**
- Derive visual mood from the Song-Beschreibung and Mood field — do not illustrate the title literally
- Abstract/atmospheric imagery preferred over narrative scenes
- Specify color palette, lighting, and texture direction
- Include style direction (e.g. "digital art", "film photography", "oil painting", "minimal graphic")
- End with: "No text on the image."
- Usable as-is with AI image generators (Midjourney, DALL-E, Stable Diffusion)
- 50-100 words

---

## Complete Example Output

Given this song file:

```
Title: Nightfall
BPM: 92 | Key: Dm | Genre: electronic | Mood: melancholic
Song-Beschreibung: A dark, atmospheric track with floating synth pads
and minimalist beats. Inspired by the silence just before sunset.
Melancholic but not sad. References: Radiohead - Everything In Its
Right Place (floating quality, space between notes).
```

Generated output written into `2026-02-Nightfall.md`:

```markdown
## Release-Texte

### YouTube
**Title:** Nightfall

**Description:**
The silence just before the light disappears — that moment where the
world holds still. Nightfall is a dark, atmospheric piece built on
floating synth pads and minimal percussion, designed to sit in that
space between calm and unease.

Melancholic, but never heavy.

BPM: 92
Key: D minor

FW004 | [Artist Name]
---
[Bandcamp] [Streaming]

### Bandcamp
**Title:** Nightfall

**Description:**
Floating synth pads drift over sparse, deliberate beats. Nightfall
captures the particular stillness that arrives at dusk — not the
absence of sound, but the quality of it. Dark and introspective,
with room to breathe.

**Tags:** electronic, ambient, dark, atmospheric, synth, melancholic, minimal, drone, slow

## Cover Art
**Prompt:**
A vast landscape at dusk, the last light fading below the horizon.
Deep shadow across the foreground, muted warm tones bleeding into
cool blue darkness. Minimal composition, high contrast, no figures
or text. Abstract atmospheric mood. Style: digital art, painterly,
wide aspect ratio. No text on the image.

**Stil-Referenzen:**
```

---

## Edge Cases

### Missing Song-Beschreibung

If the `## Song-Beschreibung` section is empty or missing:

```
Claude: "Nightfall" has no Song-Beschreibung.

        The description is the primary input for generating release texts.
        A thin description produces thin texts.

        Options:
        [1] Enter a description now (recommended)
        [2] Generate from frontmatter only (BPM, Key, Mood, Genre — minimal result)
        [3] Cancel and run /music-production:release first

User: 1
Claude: Describe the song's mood, story, and influences:
```

If the user proceeds with option 2, generate with reduced quality and add a note in the output: "Generated from metadata only — enrich Song-Beschreibung for better results."

### Existing Release Texts

If `## Release-Texte` already has content (previous generation run):

```
Claude: "Nightfall" already has generated release texts.

        Regenerate? This will overwrite the existing texts.
        [Y] Regenerate | [N] Cancel
```

The user must explicitly confirm. Never silently overwrite.

### Only BPM or Key Missing

If a frontmatter field is missing but Song-Beschreibung is present:

```
Claude: Warning: "key" is missing from frontmatter.
        Generating without key information. Run /music-production:release
        to add missing metadata, then re-run /music-production:generate.
```

Continue with generation — missing individual fields should not block the workflow entirely.

### No Songs with `status: release`

```
Claude: No songs found in 02_Vault/ with status: release.

        Run /music-production:release to prepare a song for release first.
```

---

## Notes

- Only shows songs with `status: release` (run `/music-production:release` first)
- All generated texts are written INTO the song file (`<FolderName>.md`) — not to separate files
- User should review and adjust before publishing — especially the YouTube description for tone
- Can be re-run to regenerate texts (requires explicit confirmation)
- Quality scales with the Song-Beschreibung: a rich, specific description produces markedly better texts than a generic one
- References in `## Referenzen` provide stylistic context but should not be quoted or named in release texts (unless the influence is deliberate and the user wants to mention it)
