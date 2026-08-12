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
   plus an era-specific prompt generates 3–5 second selfie-style or walk-and-talk
   clips with the character's identity preserved.
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

### 3. Generate the video clips

- Feed the anchor photo into the video engine (Seedance 2.0, Higgsfield, Kling,
  PAI Pro) as the identity reference.
- Prompt each 3–5 second clip with three blocks:
  1. **Camera/framing:** "9:16 vertical smartphone video, handheld selfie-style
     video, low-angle walk-and-talk, talking directly into the camera lens."
  2. **Performance:** "micro-gestures, natural eye blinking, subtle head tilting,
     realistic camera shake."
  3. **World:** "background filled with [era, e.g. Ancient Rome marketplace],
     historically accurate architecture, period costumes on background extras."
- Keep the character's modern outfit description verbatim in every clip prompt.
- Generate a few takes per clip; pick the ones with the most stable face.

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
4. `audio-notes.md` — voice settings, ambient layers per clip, background dialogue.
5. `edit-notes.md` — cut pacing, caption style, shake/grade notes, export settings.
6. `publish.md` — title, description, hashtags, platform notes.

## Guardrails

- Never break the Golden Rule: modern host, period-accurate world.
- Never add music or cinematic sound design; ambience only.
- Historical facts underpinning the joke/irony must be real; mark legends as such.
- The footage is clearly fictional dramatization — do not present AI clips as real
  archival or documentary material.
- Prefer concrete artifacts (prompts, scripts, files) over abstract advice.
