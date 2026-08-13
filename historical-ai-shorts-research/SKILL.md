---
name: historical-ai-shorts-research
description: "Conduct historical research on a specific event and produce a structured synopsis, voiceover script, and scene-by-scene AI prompt storyboard for a 30–60 second AI-generated vertical short video (TikTok, YouTube Shorts, Instagram Reels). TRIGGER when the user asks to research a historical event and turn it into an AI-generated short, needs a history video script/synopsis/storyboard, or wants text-to-image (Midjourney/Flux) and image-to-video (Runway/Luma/Kling) prompts for a historical scene. SKIP for long-form documentary production, pure text/article writing with no video component, or character-consistent vlog-style history videos (use ai-history-video-snippets for that)."
---

# Historical Research & Synopsis for AI Shorts (30–60s)

Step-by-step instructions for conducting targeted historical research on a
specific event and transforming the findings into a visually compelling,
dramaturgically optimized synopsis and storyboard for AI-generated short-form
videos (TikTok, YouTube Shorts, Instagram Reels).

## 1. Research Phase (Historical Fact-Gathering)

When a historical event is provided, identify and analyze the following five
core elements:

1. **Visual Core & Contrast:** What is the strongest visual contrast? (e.g., a
   majestic flagship setting sail vs. seawater pouring into open gun ports).
2. **Atmospheric & Particle Elements (AI Focus):** What environmental particles
   and lighting conditions dominate the scene? (e.g., smoke, fire, blizzards,
   torchlight, twilight, water splashes, embers).
3. **Timeline & Turning Point:** What specific sequence of events unfolding
   within minutes or hours serves as the peak dramatic climax?
4. **Human/Historical Focal Point:** Who is the key figure, group, or
   eyewitness that provides an emotional anchor for the audience?
5. **Historical Authenticity:** Key visual details required for era accuracy
   (e.g., period-accurate garments, architecture, weaponry, color palettes).

Verify facts against reputable sources when web access is available, and flag
any details that are uncertain or contested rather than inventing specifics.

## 2. Narrative Structure (30–60 Seconds)

Structure the short-form video synopsis following a three-act model:

- **Act 1: Hook & Setup (0–10 seconds)**
  - Instantly grab attention with a visual punch or a compelling voiceover hook.
  - Establish the setting, era, and atmosphere.
- **Act 2: Climax & Turning Point (10–35 seconds)**
  - The disaster or decisive moment strikes.
  - High visual motion, rising tension, and dynamic camera movement.
- **Act 3: Aftermath & Resolution (35–60 seconds)**
  - The historical impact, consequence, or a thought-provoking closing takeaway.
  - Lingering, atmospheric final shot.

## 3. Output Format

When presenting the output to the user, strictly follow this template:

```markdown
# [Event Name & Year] – AI Short Synopsis

## A. Historical Summary & Concept
- **Working Title:** [Catchy title for the short video]
- **Visual Theme:** [Color palette, lighting, mood, particle effects]
- **Historical Hook:** [Short, compelling fact explaining why this event works]

## B. Voiceover Script
A complete voiceover script written for an engaging, cinematic voice actor
(e.g., ElevenLabs).

## C. Scene-by-Scene Storyboard & AI Prompts
For each scene (3–5 scenes total):

### Scene N
- **Timestamp:** (e.g., 00:00 – 00:08)
- **Visual Action / Description:** Detailed description of the scene dynamics.
- **Voiceover:** Exact text spoken during this scene.
- **Text-to-Image Prompt (Midjourney / Flux):**
  [Detailed English prompt specifying subject, cinematic lighting, camera
  angle, era details, 8k photorealistic style --ar 9:16]
- **Image-to-Video Prompt (Runway / Luma / Kling):**
  [Motion and camera control instructions in English]
```

## 4. Prompt Engineering Best Practices

- **Style & Camera Keywords:** Use terms like `cinematic lighting`,
  `ultra-realistic`, `photorealistic`, `8k`, `historical accuracy`,
  `shot on 35mm`, `volumetric lighting`, `slow motion`,
  `dramatic atmospheric depth`.
- **Aspect Ratio:** Always set `--ar 9:16` for vertical video formats.
- **Motion Simplicity:** Keep video prompts focused on one primary camera
  motion and clear element movement (e.g., slow pan right, floating ember
  particles, heavy wind blowing through wool coats).
- **Consistency:** Reuse the same era details, color palette, and lighting
  descriptors across all scene prompts so the generated shots cut together
  coherently.