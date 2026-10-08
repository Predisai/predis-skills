# Prompting for reference-ad

Rules for the models this skill uses. Option names and allowed values always come from `describe_model`; the values below were live when this was written and may change.

## General

- The prompt carries the structure. Describe the reference's layout or beats in plain words, then put the user's product and copy into it.
- Number references in the order you pass them and give each one a job: "image 1 is the product", "image 2 is the logo".
- Never name the other brand, its product or its people in a prompt.
- Change one thing per retry.

## nano_banana_pro (static starting pick)

Options seen: `aspect` (1:1, 4:5, 16:9, 9:16), `resolution` (1K, 2K, 4K). Up to 14 image references.

Strongest at keeping the real product exact. Good with short text in one or two zones.

```
The product is exactly the one in image 1: same shape, colors, label text and proportions.
Render only what is described. No extra text, logos, badges or props.
{look} ad, {aspect}. Background: {background}.
Product from image 1 placed {position}, filling about {N}% of the frame, {angle}, {surface and shadow}, {props}.
Light: {direction and quality}. Palette: {colors with roles, hex if known}.
Text, exactly as written:
- Headline "{copy}" at {position}, {size}, {font feel}, {weight}, {case}, {color}, {alignment}.
- {next zone}
{Logo: "the logo from image 2 at {position}" or "leave {position} empty"}.
```

Spell a wordmark once: `the word "ACME" spelled A-C-M-E`.

Focused text fix: pass the take as image 1. `Change only the {zone} text to exactly "{copy}". Keep everything else identical.`

## gpt_image_2 (static, text-heavy)

Options seen: `output_size` (1024x1024, 1024x1536, 1536x1024, 768x1024, 1080x1920, 1920x1080 and larger), `quality` (low, medium, high). Up to 5 image references. No 4:5 size: pick the closest and say so.

Use when the ad has many words or 3 or more text zones. Write it like a designer's brief: grid, hierarchy, exact copy in quotes, colors as hex, safe margins. Keep the product lock line first.

## veo_3_1 (video, someone speaking)

Options seen: `aspect` (16:9, 9:16), `duration` (4, 6, 8), `audio` (on by default), `resolution`. Reference mode takes up to 3 images and locks duration to 8 s. Keyframe mode takes a first and last frame.

- Write a scene, not a caption: subject, setting, action, camera, framing, light, mood.
- Dialogue after a colon, not in quotes: `She says: My charger drawer is a crime scene.` Add `(no subtitles)`.
- About 2 to 3 short sentences per 8 seconds.
- Write the sound you want: room tone, effects tied to actions, and "no music" if none. Unwritten sound gets invented.
- Keep the speaker large in frame and the camera calm while they talk.
- Phone-shot ads: "smartphone footage, natural skin texture, slightly uneven light". Avoid "cinematic" and "8k".

## seedance_2_0 (video, no speech or several shots)

Options seen: `aspect` (1:1, 16:9, 9:16, 4:3, 3:4), `duration` (4 to 15 s, or auto), `audio`, `resolution`. Reference mode takes up to 9 images. Keyframe mode takes a first and last frame.

- Order: subject, action, scene, camera, light, sound, locks. About 40 to 110 words per shot.
- Several shots in one clip: label each one, `Shot 1:`, `Shot 2:`. One action and one camera move per shot. About 4 to 6 s per shot.
- Lock the product: `the product keeps its exact shape, label and colors, no morphing`.
- No text in the prompt for on-screen words. Generated text in video comes out garbled; it goes in the user's editor.
