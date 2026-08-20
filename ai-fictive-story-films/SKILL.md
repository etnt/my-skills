---
name: ai-fictive-story-films
description: "Adapt a short piece of source text (a micro-story, flash fiction, a scene, a paragraph, a writing prompt, a fable) into a short cinematic AI-generated fiction film using Kling AI, with faithful preservation of the source plot and characters. Produces the full production package: character anchors, a shot-broken screenplay, per-clip Kling prompts (defaulting to Custom Multi-Shot scripts), audio/voice notes, edit notes, and publishing copy. TRIGGER when the user wants to turn a short text/story/paragraph/prompt into an AI video, make a cinematic AI short film, adapt flash fiction or a scene into video, build character anchors for a fictional cast, or write Kling multi-shot prompts for a narrative story. ALSO trigger for choosing AI film tools (Kling, Nano Banana Pro, Midjourney, ElevenLabs, CapCut/Premiere) for a fictional narrative. SKIP for non-fiction/history vlogs (use ai-history-video-snippets), pure historical research shorts (use historical-ai-shorts-research), or live-action filming and long-form production."
---

# AI fictive story films (cinematic short fiction from source text)

Turn a short piece of source text into a short cinematic AI film that stays
faithful to the source — same plot, same characters, same emotional arc — just
rendered as moving pictures. The unit of input is small (flash fiction, a single
scene, a fable, a paragraph, a writing prompt); the unit of output is a
30–90 second **16:9 cinematic** film assembled from 3–5 second AI clips.

The hard-won lesson from production is that **Kling's Custom Multi-Shot mode is
the workhorse of realism** for narrative fiction: a clip built from several
short, deliberately-cut shots reads as far more cinematic and alive than one
long static take. Default to multi-shot; treat single-shot as the exception.
For the low-level grammar of multi-shot scripting, this skill leans on the
companion skill `kling.ai-custom-multi-shots-scriptwriting` — read it when you
need the exact block syntax and segmentation rules.

## The 5-layer AI stack

1. **Adaptation layer (Claude / LLM):** read the source text, extract its beats,
   cast, and voice, then break it into a shot-level screenplay that preserves the
   story faithfully.
2. **Character anchor layer (Nano Banana Pro / Midjourney):** one ultra-realistic
   reference image per named character, registered in Kling as a named
   `@Character` element so identity holds across every shot they appear in.
3. **Video layer (Kling):** each clip is generated from a per-clip storyboard
   prompt built on the five-block structure and defaulting to a Custom Multi-Shot
   script. Character elements keep faces consistent; the world carries the mood.
4. **Voice & audio layer (ElevenLabs / Kling native):** either native in-clip
   dialogue, or a laid-over narrator/character voiceover — plus a diegetic
   ambient bed. **No music score** (see house style).
5. **Editing layer (CapCut / Premiere):** clips stitched into a 16:9 cinematic
   cut with dialogue/subtitle captions, deliberate pacing, and a light film grade.

## House style rules

- **Format:** 16:9 cinematic landscape, 1920x1080, ~30–90 seconds. Pacing serves
  the story, not a feed algorithm — let beats breathe; don't machine-gun cuts.
- **Fidelity to the source is the Golden Rule.** This skill *adapts*, it does not
  rewrite. Preserve the plot, the characters, the setting, the tone, and the
  ending of the source text. You may compress (a short film can't show every
  line) and you may translate prose into concrete visuals, but never invent a new
  plot, change who a character is, or "improve" the ending. If the source is
  ambiguous, make the smallest visual choice that stays true to the text, and
  note the assumption rather than embellishing.
- **Cinematic camera language:** composed framing, intentional camera moves
  (slow push-ins, dollies, tracking, rack focus), shallow depth of field, motivated
  lighting. This is a film, not a phone vlog — steadier and more designed than the
  history-vlog style, but still grounded and physically plausible.
- **Sound design:** diegetic ambience and effects only — room tone, wind, rain,
  footsteps, distant voices, the world of the story. **Do not add a music score
  or cinematic whooshes;** the emotion comes from performance, image, and silence.
- **Captions:** clean cinematic subtitles for dialogue (or narrator lines),
  bottom-centered, unobtrusive — not influencer-style kinetic captions.

## Workflow

### 1. Read and break down the source text

- Read the source material closely. Write a one-paragraph **faithful synopsis**
  and a beat list: the 3–6 story beats that must survive adaptation (setup,
  turn, climax, resolution — whatever the piece actually has).
- Extract the **cast**: every named or clearly-identified character, with the
  physical and wardrobe details the text gives. Where the text is silent, choose
  restrained, tone-appropriate specifics and mark them as adaptation choices.
- Identify the **point of view and voice**: is the story carried by character
  dialogue, by a narrator, or both? This decides the audio approach in step 4.
- Decide the **audio approach per project** (pick one and stay consistent):
  - *Native dialogue:* characters speak in-clip via Kling native audio
    (lip-synced, simplest, voice may drift between generations).
  - *Narrator voiceover:* generate near-silent performances and lay an
    ElevenLabs narrator (or character) voice over them in the edit — best voice
    consistency, ideal for prose-driven or literary source text.
  - You may combine them across a film (narrator frames it, characters speak
    inside scenes) — but never stack both inside the *same clip*.

### 2. Establish the character anchors (once per character, reuse forever)

- For each named character, fix a **verbatim character description**: age, face,
  hair, skin, build, and signature wardrobe consistent with the source and era.
  Reuse this exact text everywhere.
- Generate an ultra-realistic **anchor image** per character (front, 3/4, profile;
  neutral and expressive faces help identity hold).
- In Kling, register each anchor as a named character element and reference it
  with its exact tag (e.g. `@Mara`, `@TheStranger`) in every prompt where the
  character appears. **Once the tag is used, never re-describe core features**
  (age, face, hair) — describe only actions, wardrobe state, and lighting on the
  character. This is what keeps a multi-character cast from morphing shot to shot.

### 3. Write the shot-broken screenplay

- Turn the beat list into a **shotlist**: for each beat, one or more clips, each
  3–5 seconds. Aim for coverage that tells the story clearly — establishing
  shot, the performances, the key objects, the turn.
- Assign each line of dialogue (or narration) to a specific clip. Keep the
  source's words where the source gives words; where you must paraphrase for
  length, stay in the character's/narrator's voice and note it.
- Keep world details (location, era, props, weather, palette) **continuous and
  repeated** across prompts so the clips cut together as one coherent film.

### 4. Generate the video clips (storyboard prompts)

Write **one storyboard file per clip** in `story-board/`. Default to a Custom
Multi-Shot script (`clip-XX-multishot.txt`); keep a single-shot fallback
(`clip-XX.txt`) only for very short beats. Use the five-block structure — this
block syntax stops Kling from confusing camera directions with environment or
dialogue:

```
[Camera & Action]: Shot type + camera movement; @Character's exact action,
gaze direction, framing, and micro-expressions (end with "natural lip sync,
subtle eye blinking ... as they speak" when they have a line).
[Environment]: Background elements and their moving parts (crowd, smoke, wind,
rain, traffic), true to the story's world, with proximity to camera.
[Lighting & Atmosphere]: Time of day, light quality/temperature, and the
emotional tension of the beat.
[Native Audio Dialogue]: @Character + delivery direction (tone, pacing, volume)
+ a voice-quality anchor (see below) + the line VERBATIM in quotation marks.
Omit this block entirely for clips using a laid-over ElevenLabs voiceover.
[Background Soundscape]: 3–4 diegetic audio layers separated by commas — no
music, no whooshes.
```

**Prefer Custom Multi-Shot for almost every clip.** The cut itself is the
realism cue. One shot = 3–5 seconds; speech runs ~2–3 words per second, so split
a line at natural sentence breaks into sequential quoted strings, one per shot —
never reword or reorder the source. Read
`kling.ai-custom-multi-shots-scriptwriting` for the full segmentation and tagging
rules. Key points:

- Tag `@Character` in the `[Camera & Action]` of **every** shot where the
  character is visible or audible. For cutaways (camera on the scene, not the
  face), use the off-screen voiceover form and keep a piece of the character in
  frame (a hand/sleeve at the edge).
- Keep environment, lighting, and soundscape continuous across the shots so the
  cut reads as one unbroken scene. A–B–A (face → cutaway → face) works well for
  reveal and reaction beats; a slow build of tighter shots works for a rising
  climax.
- Kling dashboard: Multi-Shot → Custom Multi-Shot → paste each shot's blocks
  into its own numbered window → set per-shot duration sliders (3–5s) → Native
  Audio master switch ON only if you're using native dialogue → Generate.

**Voice-quality anchor (prevents metallic/raspy voices):** Kling's native audio
tends to render whispered, elderly, or "gravelly" voices with metallic digital
artifacts. When using native dialogue, append the SAME voice-quality anchor to
EVERY `[Native Audio Dialogue]` line: e.g. "in a warm natural [gender/age] voice,
close studio condenser microphone, rich organic tone, cinematic acoustic clarity,
smooth clean vocals, never metallic or raspy". Keep the structure — (1) voice
character, (2) recording-quality anchor, (3) explicit negatives — and use only
one delivery cue (don't stack "gravelly" + "raspy" + "whisper"); if a clip still
comes out metallic, soften "whispers" to "speaks very quietly" and re-roll (audio
varies between seeds with identical prompts).

Generate a few takes per clip and keep the ones with the most stable faces and
truest performance.

### 5. Voice and sound

- If using a narrator or a consistent character voice, clone/fix one ElevenLabs
  voice per speaker and reuse it across the whole film. Direct the delivery to
  match the source's tone (intimate, ominous, wry, matter-of-fact).
- Build a diegetic ambient bed per clip that belongs to the story's world. No
  music. No cinematic whooshes. Silence is a legitimate, powerful choice.

### 6. Edit and publish

- Assemble in CapCut/Premiere at 16:9, 1920x1080. Cut for story rhythm, not feed
  speed; hold on the beats that matter.
- Add clean cinematic subtitles for dialogue/narration, bottom-centered.
- Apply a light, consistent film grade (a single look across all clips helps them
  read as one film). Avoid over-stylized LUTs that fight the story.
- Export 16:9, 1920x1080, H.264 MP4. Write a title, logline, synopsis, and
  hashtags/description suited to YouTube and short-film-friendly platforms, and
  credit the source text if it isn't the user's own.

## Output package

1. `source.md` — the original text (or a pointer to it), the faithful synopsis,
   and the beat list, with any adaptation choices flagged.
2. `characters.md` — one verbatim description + anchor-image prompt per character,
   and each character's Kling `@tag`.
3. `screenplay.md` — the shot-broken screenplay: beats mapped to clips, with each
   clip's dialogue/narration line and the chosen audio approach.
4. `shotlist.md` — table: clip #, beat, characters in shot, narration/dialogue
   line, camera + performance + world summary, duration, take notes.
5. `story-board/clip-XX-multishot.txt` — one generation-ready Custom Multi-Shot
   script per clip (the default), plus `clip-XX.txt` single-shot fallbacks in the
   five-block format. Native-dialogue clips carry the voice-quality anchor.
6. `audio-notes.md` — voice path (native vs ElevenLabs), voice settings per
   speaker, and the ambient layers per clip.
7. `edit-notes.md` — cut rhythm, subtitle style, grade/look notes, export settings.
8. `publish.md` — title, logline, synopsis, hashtags/description, source credit.

## Guardrails

- Faithful adaptation first: preserve the source's plot, characters, tone, and
  ending. Compress and visualize, never rewrite or invent.
- Keep each character's verbatim description and `@tag` consistent across every
  prompt so a multi-character cast stays recognizable.
- Diegetic sound only — no music score, no whooshes.
- Prefer Custom Multi-Shot; reserve single-shot for very short beats.
- The footage is clearly a fictional AI dramatization — don't present it as real
  footage, and credit source text you didn't write.
- Prefer concrete artifacts (prompts, screenplay, files) over abstract advice.
