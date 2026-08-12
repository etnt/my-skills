---
name: irish-backing-track
description: Create a traditional Irish backing track in MIDI format from ABC notation of a tune, using AI tools, for playing along on tin whistle. Use when the user wants to turn an ABC tune (reel, jig, hornpipe, etc.) into a multi-track MIDI accompaniment with traditional Irish instruments (bouzouki, guitar, bodhrán, fiddle, bass) for import into Garageband or another DAW.
---

# Irish Backing Track from ABC Notation

Generate a multi-track MIDI backing track (no melody, or melody on a mute-able track) from ABC notation, suitable for importing into Garageband to practice tin whistle along with.

## Workflow overview

1. **Analyze the ABC tune** — key, mode, rhythm type, structure.
2. **Arrange the accompaniment** — chords, voicings, rhythm patterns per instrument.
3. **Convert to MIDI** — via ABC→MIDI tools or a small script.
4. **Import & polish in Garageband.**

## Stage 1: Analyze the tune

From the ABC header and melody, extract:

- `K:` field → key/mode. Irish tunes are often **modal** — don't trust the key signature alone:
  - **D major / B minor / E dorian / A mixolydian** all share two sharps; check the final note and chord tendencies.
  - **G major / E minor / A dorian / D mixolydian** share one sharp.
- `M:` field → meter and tune type:
  - 4/4 → reel or hornpipe (hornpipe often swung/dotted)
  - 6/8 → jig; 9/8 → slip jig; 12/8 → slide
  - 3/4 → waltz/mazurka; 2/4 or 2/2 → polka / march
- `L:` default note length, `Q:` tempo.
- Structure: parts (A, B…), repeat marks `|: :|`, typical form AABB.

Ask the user for the ABC if not provided; confirm tune type and tempo (reels ~100–120 half-note bpm, jigs ~110–130 dotted-quarter bpm).

## Stage 2: Harmonize and arrange

### Chord rules for Irish trad

- Keep harmony **sparse and modal** — avoid functional V7–I cadences; prefer I–♭VII–IV movement (in D: D–C–G).
- One chord per bar at most; sometimes one per two bars.
- Default progressions (one chord per bar, per 8-bar part):
  - D major: | D | D | G | D | D | G | C D | D |
  - D mixolydian: | D | C | D | C | D | C | G C | D |
  - E dorian: | Em | Em | D | Em | Em | C | D | Em |
  - A dorian: | Am | Am | G | Am | Am | G | Em G | Am |
  - G major: | G | G | C | G | G | C | D C | G |
- Derive chords from the melody's strong beats; **show the chord chart to the user for approval** before generating MIDI.

### Instrumentation (one MIDI track/channel each)

| Track | Instrument | GM program | Role |
|---|---|---|---|
| 1 | Melody (mute-able) | 78 Whistle (or 73 Flute) | Reference melody |
| 2 | Bouzouki / Guitar | 25 Steel Gtr (or 26) | Chord strums / arpeggios |
| 3 | Bass | 33 Acoustic Bass | Root & fifth on beats |
| 4 | Bodhrán | Ch 10 percussion, keys 35/38/46 | Rhythm |
| 5 | Fiddle/Pad (optional) | 40 Violin / 89 Pad | Sustained chord tones |

### Rhythm patterns per tune type

- **Reel (4/4)**: bodhrán low hit on 1 & 3, triplet flourish on 4 every 2nd/4th bar; bouzouki strums eighth notes emphasizing 1 & 3.
- **Jig (6/8)**: bodhrán LOW-hi-hi LOW-hi-hi; guitar: bass note + two strums per bar.
- **Hornpipe**: swung eighths, heavier on 1 & 3.
- **Waltz (3/4)**: bass on 1, strums on 2 & 3.
- **Polka (2/4)**: driving eighth-note strums, bass alternating root/fifth.

Use open, droney voicings (root + fifth + octave, **omit thirds**) — keeps it modal and trad-sounding, GDAD-bouzouki feel.

## Stage 3: Generate the MIDI

**Route A (recommended): ABC → MIDI**

1. Compose an arrangement ABC file with the melody plus separate backing voices:
   ```abc
   X:1
   T:The Silver Spear (backing)
   M:4/4
   L:1/8
   Q:1/2=110
   K:D
   V:1 name="Melody" clef=treble
   %%MIDI program 78
   V:2 name="Bouzouki" clef=treble
   %%MIDI program 25
   V:3 name="Bass" clef=bass
   %%MIDI program 33
   V:1
   "D"faaf defe|"G"d2 ...
   V:2
   [D2A2d2][D2A2d2] [D2A2d2][D2A2d2]|...
   V:3
   D,2 A,,2 D,2 A,,2|...
   ```
   Write repeats out fully (AABB), and add a 2-bar bodhrán-tap count-in at the start.
2. Convert: `abc2midi tune.abc -o backing.mid` (install via `brew install abcmidi`, or use an online converter at abcnotation.com / mandolintab.net).
3. For bodhrán, add a percussion voice with **both** `%%MIDI channel 10` **and** `%%MIDI drumon` (abc2midi 5.x does not route drum voices to channel 10 automatically — verified). Drum pitches: `B,,`=35 kick, `D,,`=38 snare, `^F,,`=42 hat, `E,,`=47 low tom (use for the LOW tipper hit).
4. Verify: file opens, correct tempo, one track per voice. Re-open with `midi2abc` or a DAW to sanity-check.

If abcmidi isn't available, have an AI tool write a small Python script using `music21` or `mido` to emit the same tracks programmatically — inspectable and re-runnable.

**Route B: Pure AI chat generation**

Prompt Claude/ChatGPT with the template below to produce the arrangement ABC or a mido script. Never trust raw binary MIDI from a chat model — always go through ABC or a script you can inspect.

```
You are an expert arranger of traditional Irish music. Here is a tune in
ABC notation: [PASTE ABC]. It is a [reel/jig/...] in [key/mode].

Create an accompaniment arrangement as ABC with separate V: voices for:
1) original melody (program 78 whistle),
2) bouzouki-style chord backing (program 25; sparse modal voicings,
   omit thirds; [rhythm pattern for tune type]),
3) acoustic bass (program 33; root/fifth),
4) optional sustained fiddle/pad.
Use sparse modal Irish harmony (I-bVII-IV motion, max one chord per bar,
no V7 cadences). Write out the full AABB structure without repeat signs.
Output only valid ABC that abc2midi can convert.
```

**Route C: AI music services**

AIVA, Soundraw, Chordify etc. mostly export audio (not clean multitrack MIDI) and model trad rhythm poorly. Prefer Routes A/B.

## Stage 4: Sounds — realism matters

**MIDI contains no sound** — only note instructions plus a GM program number. Garageband's built-in library has no bouzouki, bodhrán, whistle, or pipes; its generic GM sounds will always sound fake. For realistic backing tracks, the user must install sampled-instrument Audio Unit plugins (Garageband loads AUs automatically) and map each track to a real instrument.

### Recommended instrument sources (user has indicated cost is not an issue)

- **Best Service "Celtic ERA"** (Eduardo Tarilonte) — the core purchase; purpose-built for this: Irish bouzouki, bodhrán (multiple, with tipper/bass articulations), tin & low whistles, uilleann pipes, Celtic harp, guitars, cittern, bowed strings. One library covers most of a trad band.
- **Impact Soundworks "Ventus Winds: Tin Whistle"** — deeply sampled whistle (also low whistle); use for the melody reference track.
- **Irish flute** — use the wooden flute in Celtic ERA, or a dedicated flute library with breathy/reedy articulations.
- **Fiddle** — any expressive solo violin (e.g. NI Stradivari, Embertone); realism comes from written-in ornamentation (see below), not the patch alone.
- **Concertina** — a good accordion/concertina patch (e.g. Best Service Accordions) works; write push-pull rhythmic phrasing into the part.
- **Free fallbacks** — Spitfire LABS, Plogue sforzando + free Celtic SFZ packs, if the user wants to start at zero cost.

### Alternatives to Celtic ERA (second-best options)

- **Native Instruments "Spotlight Collection: Ireland"** (Kontakt) — the strongest rival: uilleann pipes, Irish flute, tin whistle, fiddle, concertina, bodhrán, harp, and plucked strings, all sampled with trad playing styles and built-in ornament articulations. Excellent choice if the user already owns Kontakt; compare its bouzouki/bodhrán depth against Celtic ERA before choosing.
- **Best Service "Ethno World 6"** (Marcel Barsotti) — a huge world-instrument library whose European section covers whistles, bagpipes, fiddles, frame drums, and bouzouki-family instruments. Broader than Celtic ERA but less trad-session-focused; good if the user also wants non-Irish folk sounds.
- **Best Service "ERA II Medieval Legends"** (also Tarilonte) — cittern, lutes, frame drums, early winds; the folk textures overlap well with trad backing, though it lacks modern session staples.
- **Mix-and-match build** — for a tailored rig: Orange Tree Samples "Evolution Mandolin" (mandolin is the bouzouki's close cousin and covers rhythmic backing well), Ilya Efimov tin/low whistle, any frame-drum library for bodhrán, plus an expressive solo violin for fiddle. More flexible, but more work to blend.

### The trad instrument palette (choose per arrangement)

Offer the user a choice of ensemble from these roles before arranging:

| Instrument | Role in backing track | Authoring tips (write into the ABC/MIDI) |
|---|---|---|
| Tin whistle | Melody reference (mute-able) | Slight breath: velocity variation; occasional cut grace notes |
| Uilleann pipes | Melody double / drone track | Add a constant D drone (root+fifth) on a separate track; chanter plays melody an octave down or in unison |
| Irish fiddle | Melody double, or sustained chord pads | Write ornaments: cuts/rolls as 32nd-note grace notes, slight swung bowing on jigs |
| Irish flute | Melody double (warm, low) | Long phrases, few tongued notes; glides between notes |
| Concertina | Rhythmic melody double / chord stabs | Short staccato chord punches off the beat |
| Irish bouzouki | **Core of the backing**: chord strums/arpeggios | GDAD-style open voicings, no thirds, bass-note + strum jig pattern |
| Bodhrán | Rhythm | LOW tipper hit on beat 1, stick pattern; add triplet "rolls" at part boundaries |

A good default practice ensemble: **bouzouki + bass + bodhrán + optional fiddle/pipes pad**, melody on whistle (mute-able). Offer alternatives: "add pipes drone?", "fiddle doubling the melody?", "concertina stabs instead of strumming?"

### Realism techniques to write into the MIDI

- **Velocity humanization**: vary note velocities ±10–20%, accent beat 1.
- **Timing**: keep it tight — trad groove comes from accent placement, not sloppiness. Optional tiny swing on hornpipes.
- **Ornaments**: add cuts (upper grace note, 1/32) on long melody notes; rolls at phrase peaks.
- **Drones**: pipes/fiddle drone on tonic+fifth transforms the sound instantly.
- **Bodhrán dynamics**: accent pattern follows the tune's phrases, not a flat loop — vary every 4 bars, add a fill before each part repeat.

### Garageband import

1. Drag the `.mid` onto the track area — Garageband creates one track per MIDI channel.
2. Replace each track's instrument with the corresponding AU (Celtic ERA bouzouki, bodhrán, etc.). For the bodhrán track, either use Celtic ERA's bodhrán (played via its key mapping — adjust MIDI note numbers in the ABC drum voice to match) or a kit with a deep low tom.
3. Set tempo from the ABC `Q:` value if it didn't carry over.
4. Mute/lower the melody track when playing along; pan bodhrán slightly off-center, bouzouki opposite, drones center.
5. Optionally loop the tune 3×, as in session practice.

## Checklist before delivering

- [ ] Key/mode verified against the melody's final note, not just key signature
- [ ] Chord chart approved by user
- [ ] Rhythm pattern matches tune type
- [ ] Each instrument on its own MIDI channel/track; percussion on ch 10
- [ ] Tempo set; count-in included; AABB written out
- [ ] MIDI verified by re-opening (or `midi2abc`) before handing to user

