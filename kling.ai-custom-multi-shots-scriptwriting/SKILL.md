# Skill Overview & Purpose

This skill enables the AI to architect, break down, and format a continuous
cinematic scene into a modular, multi-clip text prompt optimized for
Kling.ai’s Custom Multi-Shot interface. The primary objective is to
maintain perfect visual, character (@Character), and audio continuity
while executing professional film-style editing (cuts, camera movement,
and pacing) in a single generation.

## Core Execution Principles

### The Golden Rule of Segmentation

Kling.ai limits single-shot generations
to 5 seconds by default. A continuous text prompt with multiple actions
will often experience "visual drift" or cut off. When building a
Custom Multi-Shot script:

- Break the narrative into logical cinematic beats (Shots).
- Limit each Shot to 3 to 5 seconds of visual duration.
- Target 1 text clause per action per Shot.

### Element Tagging (@) Preservation

* If a fixed character asset is used, the character's exact identifier
  (e.g., @Reginald) must appear in the [Camera & Action] block of every
  single Shot where they are visible or audible.
  
* Do not re-describe the character's core features (e.g., "70-year-old man")
  once the element tag is used. Focus strictly on clothing shifts,
  lighting on the skin, and specific actions.

### Native Audio & Dialogue Routing

* Kling.ai processes speech dynamically. Dialogue inside quotation marks ""
  must be mapped sequentially across shots.
  
* Audio Pacing Rule: A human speaks at roughly 2–3 words per second.
  Ensure the length of the string inside "" matches the duration allotted
  to that specific Shot.

* For off-camera voiceovers, use the descriptor: [Native Audio Dialogue]: @Character's voice
  continues over the clip as an off-screen voiceover, whispering close to the microphone: "..."

## Mandatory Prompt Structure Per Shot

Every individual Shot generated for the Multi-Shot configuration must
strictly adhere to this block-syntax to prevent Kling from confusing
environmental descriptions with camera directions or spoken dialogue:

```
[Camera & Action]: [Specify camera type/movement: e.g., Medium close-up shot, steady tracking pan]. [Describe @Character's exact physical action, gaze direction, framing, and facial micro-expressions].

[Environment]: [Describe the background elements, dynamic background moving parts like smoke or wind, and proximity to the camera].

[Lighting & Atmosphere]: [Define time of day, cinematic lighting quality, color grading temperature, and overall emotional tension].

[Native Audio Dialogue]: @Character [describe tone, accent, volume, or pacing adjustment]: "[Insert exact spoken text string here]".

[Background Soundscape]: [List 3-4 distinct audio layer cues separated by commas, excluding the dialogue].
```

## Prompt Engineering Vocabulary (Kling-Optimized)

To achieve maximum compliance from the Kling Omni 3.0 model,
utilize these highly-responsive keywords within the structural blocks:

| Category  | High-Performance Kling Keywords  |
|-----------|----------------------------------|
| Camera Style  | Handheld selfie-style camera, Dolly zoom, Shallow depth of field (blurred background), Cinematic medium close-up, Static tripod frame. |
| Character Control | Natural lip sync, Subtle eye blinking, Micro-expressions, Gaze locked to the lens, Deliberate hand gesture. |
| Audio Mechanics | Hushed confiding tone, Theatrical gravelly whisper, Brisk speech pacing, Breathy pause, Trembling elderly accent. |
| Atmosphere | Quietly tense, Historically authentic, Volumetric morning mist, Baltic sun flare, Holding its breath. |


## Workflow Execution Steps for Users

When applying this skill inside the Kling.ai dashboard, execute the following steps:

1. Toggle Multi-Shot: Look below or adjacent to the main prompt input area and check the Multi-Shot box.
2. Switch to Custom: Click on Custom Multi-Shot to transition from automated AI cuts to manual script injection.
3. Populate Shot Boxes: Click the + icon to generate new script windows (Shot 1, Shot 2, Shot 3). Copy and paste each structured block output into its matching numbered window.
4. Allocate Durations: Manually set the individual time slider for each shot box (e.g., Shot 1 = 3s, Shot 2 = 4s, Shot 3 = 3s).
5. Verify Voice Synthesis: Ensure the Native Audio master switch is turned ON (Green) before pressing the main Generate button.

