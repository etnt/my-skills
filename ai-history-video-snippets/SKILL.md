---
name: ai-history-video-snippets
description: "Plan, script, and produce short AI-generated 'time-travel vlogger' history videos in the style of the 'Chloe vs History' channel: a consistent modern-day character films selfie-style vertical clips in historical eras using a multi-layer AI pipeline (LLM script, character anchor image, identity-referenced AI video, cloned voice, ambient sound design, mobile-vlog edit). TRIGGER when the user asks to create an AI-generated video, a history short/reel/TikTok, a time-travel vlog, a 'Chloe vs History'-style video, a character anchor/reference sheet, or identity-consistent AI video clips. ALSO trigger for choosing AI video tools (Seedance, Higgsfield, Kling, PAI Pro, Nano Banana Pro, Midjourney, ElevenLabs, CapCut) or building a multi-layer AI video pipeline. SKIP for live-action filming, long-form documentary production, or pure text/article writing with no video component."
---

# AI history video snippets (Chloe vs History style)

Produce short-form, AI-generated "time-travel vlog" videos: a consistent modern-day
character appears to film themselves, smartphone-in-hand, inside historical eras —
delivering modern commentary directly to camera. The style's power comes from the
contrast between the modern "tourist" and the historically accurate world around
them. This skill produces the full production package (anchor character, script,
prompts, edit notes) and, when tools are available, the assets themselves.

## The 5-layer AI stack

1. **Script layer (Claude / LLM):** historical narrative, scene-by-scene visual
   instructions, and period-accurate background dialogue.
2. **Image anchor layer (Nano Banana Pro / Midjourney):** an ultra-realistic
   character reference sheet / base photo that keeps the host identical in every
   shot.
3. **Video layer (Seedance 2.0 / Higgsfield / Kling / PAI Pro):** the anchor photo
   (registered as a named @Character element in Kling) plus a per-clip
   story-board prompt generates 3–5 second selfie-style or walk-and-talk clips
   with the character's identity preserved. Prompts use a five-block structure
   (camera/action, environment, lighting, native audio dialogue, soundscape);
   clips default to Custom Multi-Shot scripts (multi-shot reads as markedly
   more realistic), with single-shot prompts only for very short lines.
4. **Voice & audio layer (ElevenLabs):** a cloned/consistent voice for the modern
   commentary, plus period-accurate ambient sound (market chatter, horse hooves,
   ancient languages) layered underneath for immersion.
5. **Editing layer (CapCut / Premiere):** clips stitched into 9:16 vertical video
   with mobile-vlog captions, realistic camera shake, and quick pacing.

## House style rules

- **Format:** 9:16 vertical smartphone footage, short videos (~15–60s), fast
  pacing: 3–4 cuts per 15 seconds.
- **The Golden Rule of the anchor character:** the host ALWAYS wears modern
  clothes, a modern hairstyle, and optionally modern accessories (watch,
  headphones) — in every era. The anachronism is the point; never dress the host
  in period costume. Background extras, architecture, and props are
  period-accurate.
- **Camera language:** handheld selfie-style, walk-and-talk, low angles, talking
  directly into the lens, realistic camera shake, micro-gestures, natural eye
  blinking, subtle head tilts. It must look like it was shot on a phone, not a
  film set.
- **Sound design:** strip any AI-generated music. Use ONLY diegetic ambient sound
  (wind, distant chatter, footsteps, era-appropriate noise) so the footage feels
  filmed live. The voice is the character speaking to their phone.
- **Captions:** auto-captions in an influencer-style font, mobile-vlog placement.
- **The two narrative formulas** (pick one per video):
  - *The Escape variant:* the host visits a terrible era and judges it harshly,
    like a bad review ("Tudor London, 2/10, smells horrible, get me out of here").
  - *The Historical Irony variant:* the host tries to warn people about a disaster
    they cannot stop (warning Titanic passengers, Pompeii citizens, etc.).

## Workflow

### 1. Establish the anchor character (do this once, reuse forever)

- Generate (or use a real photo of) an ultra-realistic base image of the host.
- Fix a verbatim character description: age, face, hair, skin, build, and the
  signature modern outfit + accessories. Reuse this exact text in every prompt.
- Produce a character reference sheet (front, 3/4, profile; neutral and expressive
  faces) so identity stays consistent across all clips and all future videos.

### 2. Write the script and scene plan

- Pick one era/event with built-in drama or irony. Verify the historical facts
  (dates, places, customs) — the comedy/horror must rest on real history.
- Choose the narrative formula: Escape or Historical Irony.
- Write the host's commentary in modern, casual vlogger speech: short sentences,
  present tense, reactive ("okay so— that smell is REAL"), talking to the viewer.
- Plan scene-by-scene: 3–4 clips per 15 seconds; each clip gets its own video
  prompt. Include period-accurate background dialogue/details for the ambient mix.
- Hook in the first 1–2 seconds: the host mid-situation ("So I'm in Pompeii and
  the mountain is doing something weird...").

### 3. Generate the video clips (story-board prompts)

- Feed the anchor photo into the video engine (Seedance 2.0, Higgsfield, Kling,
  PAI Pro) as the identity reference. In Kling, register the anchor as a named
  character element and reference it with its exact tag (e.g. `@Reginald`) in
  every prompt — once the tag is used, never re-describe core features (age,
  face, hair); describe only actions, clothing state, and lighting on the
  character.
- Write **one story-board file per clip** (`story-board/clip-XX.txt`), using
  five structured blocks — this block syntax stops the model from confusing
  camera directions with environment or dialogue:

  ```
  [Camera & Action]: Shot type + camera movement; @Character's exact action,
  gaze direction, framing, and micro-expressions (end with "natural lip sync,
  subtle eye blinking ... as he/she speaks").
  [Environment]: Background elements and their moving parts (crowds, smoke,
  wind, banners), period-accurate, with proximity to camera.
  [Lighting & Atmosphere]: Time of day, light quality/temperature, emotional
  tension of the beat.
  [Native Audio Dialogue]: @Character + delivery direction (tone, pacing,
  volume) + a voice-quality anchor (see below) + the clip's narration
  line VERBATIM, in quotation marks.
  [Background Soundscape]: 3–4 diegetic audio layers separated by commas —
  no music, no whooshes.
  ```

- **Voice-quality anchor (prevents metallic/raspy voices):** Kling's native
  audio tends to render elderly, whispered, or "gravelly" voices with
  metallic digital artifacts. Mitigate by appending the SAME voice-quality
  anchor to EVERY `[Native Audio Dialogue]` line in every clip:
  "in a deep warm natural elderly male voice, close studio condenser
  microphone, rich organic tone, cinematic acoustic clarity, smooth clean
  vocals, never metallic or raspy". Adapt the voice description to the
  character but keep the structure: (1) voice character, (2) recording-quality
  anchor (studio microphone, acoustic clarity), (3) explicit negatives
  ("never metallic or raspy"). Also avoid stacking texture cues
  ("gravelly" + "raspy" + "whisper") — one delivery cue only; if a clip
  still comes out metallic, soften "whispers" to "speaks very quietly" and
  re-roll (audio quality varies between seeds with identical prompts).
- **Prefer multi-shot whenever possible.** Custom Multi-Shot clips read as
  noticeably more realistic than one static selfie take — the cut itself is
  the realism cue. Default to building a **Custom Multi-Shot** script
  (`story-board/clip-XX-multishot.txt`) for every clip; reserve single-shot
  prompts only for very short clips (one short sentence). One shot =
  3–5 seconds max; speech runs ~2–3 words per second, so split the narration
  line at natural sentence breaks across the shots:
  - Split the line at natural sentence breaks into sequential quoted strings,
    one per shot; never reword or reorder the script.
  - Tag @Character in the `[Camera & Action]` of EVERY shot where the character
    is visible or audible. For cutaway shots (camera turned to the scene), use
    the off-screen form: "@Character's voice continues over the clip as an
    off-screen voiceover, whispering close to the microphone: "..."" — and keep
    a piece of the character in frame (sleeve/hand at the frame edge).
  - Keep environment, lighting, and soundscape continuous across the shots so
    the cut reads as one unbroken take; A–B–A (face → cutaway → face) works
    well for irony beats.
  - Kling dashboard: Multi-Shot → Custom Multi-Shot → paste each shot's blocks
    into its own numbered window → set per-shot duration sliders (3–5s) →
    Native Audio master switch ON → Generate.
- Keep the character's modern outfit description verbatim in every prompt
  (or in the @Character element definition); keep era detail in the world.
- Generate a few takes per clip; pick the ones with the most stable face.
- **Two voice paths — pick one per video:** (a) native in-generation dialogue
  via `[Native Audio Dialogue]` (lip-synced, simplest, voice may vary between
  generations), or (b) generate silent performances and lay the cloned
  ElevenLabs voice over them in the edit (best cross-episode voice
  consistency). Never mix both in the same clip.

### 4. Voice and sound

- Clone or fix one ElevenLabs voice for the host; keep it consistent across all
  videos. Delivery: casual, reactive, slightly breathless "on location" energy.
- Build the ambient bed per clip: era-correct crowd noise, animals, weather,
  distant period dialogue. No music. No cinematic whooshes.

### 5. Edit and publish

- Assemble in CapCut/Premiere: clips cut tight to the voice, 3–4 cuts per 15s.
- Add or enhance camera shake in the edit if the AI footage is too smooth.
- Auto-caption with an influencer-style font; place captions like a phone vlog.
- Grade lightly toward "phone camera" realism — avoid cinematic color grades.
- Export 9:16, 1080x1920, H.264 MP4. Write title/description/hashtags for TikTok,
  YouTube Shorts, and Reels.

## Output package

1. `character.md` — verbatim character description + reference-sheet prompts.
2. `script.md` — chosen formula, narration lines, fact list with sources.
3. `shotlist.md` — table: clip #, narration line, full video prompt (camera +
   performance + world blocks), duration, take notes.
4. `story-board/clip-XX-multishot.txt` — one generation-ready Custom
   Multi-Shot script per clip (the default; multi-shot is more realistic),
   plus `clip-XX.txt` single-shot fallbacks in the five-block format
   ([Camera & Action] / [Environment] / [Lighting & Atmosphere] / [Native
   Audio Dialogue] / [Background Soundscape]). Every dialogue line carries
   the voice-quality anchor against metallic/raspy synthesis.
5. `audio-notes.md` — voice settings, ambient layers per clip, background dialogue.
6. `edit-notes.md` — cut pacing, caption style, shake/grade notes, export settings.
7. `publish.md` — title, description, hashtags, platform notes.

## Guardrails

- Never break the Golden Rule: modern host, period-accurate world.
- Never add music or cinematic sound design; ambience only.
- Historical facts underpinning the joke/irony must be real; mark legends as such.
- The footage is clearly fictional dramatization — do not present AI clips as real
  archival or documentary material.
- Prefer concrete artifacts (prompts, scripts, files) over abstract advice.
