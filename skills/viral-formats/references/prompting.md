# Prompting the viral-format models

Starting picks: `veo_3_1` / `veo_3_1_fast`, `seedance_2_0` / `seedance_2_0_fast`, `kling_3_standard`, `wan_2_2_i2v_fast`, `nano_banana_pro` / `nano_banana_2`, `dreamactor_m2`. Option names (aspect, duration, audio, resolution) and reference slots come from `describe_model`, never from this file.

## Every video model

- Describe a scene, not a caption: subject, action, place, camera, light, sound.
- One main action per shot, with a visible end ("skids to a stop", "the slice falls").
- One camera move per shot, with a start and an end.
- Replace vague words with things a camera could detect. "Cinematic" becomes a lens, a move and a light. "Epic" becomes scale and distance. Drop "8K" and "masterpiece".
- Something must not appear: describe what is there instead.
- Same prompt often gives near-identical takes. For real variety, change the wording.

## Veo (`veo_3_1`, `veo_3_1_fast`)

- The pick when sound matters. Write all four sound layers: dialogue, ambience, effects, music (or "no music"). Unwritten ambience gets invented.
- Dialogue after a colon, not in quotes: `The baby says: Nobody needs a five-step skincare routine.` Add `(no subtitles)`.
- About two short sentences per 8 s. More gets rushed or cut.
- Several speakers: tie each line to a visual tag ("the man in the toga replies: ...").
- Clean mouth movement: speaker large in frame, calm camera during the line.
- Spell hard names the way they sound.
- With reference images the duration is fixed at 8 s. A first frame and last frame mode also exists.
- Draft on `veo_3_1_fast`, finish on `veo_3_1` with the same prompt.

## Seedance (`seedance_2_0`, `seedance_2_0_fast`)

- Order: subject and action first, then place, camera, light and style, sound, then locks. Keep it to about 40 to 110 words per shot.
- Multi-shot in one clip: label every cut `Shot 1:`, `Shot 2:`. Give each shot one action, one move and its sound. Budget 4 to 6 s per shot. Unlabeled long prompts render as one take.
- A look that must hold across shots is stated once for the whole clip and kept word for word.
- End each shot on a completed beat so the cut does not land mid-action.
- The fast tier follows multi-shot and slow-motion less reliably. Use `seedance_2_0` for the final.
- Native audio: name the specific sounds per shot; they anchor timing.

## Kling (`kling_3_standard`)

- Shape: subject, one action with an end, setting, then `Camera: <shot size>, <one move from X to Y>`, light, style.
- Physical cause and effect beats a list of small moves.
- Takes a first frame and an optional last frame image. With a first frame, describe only motion and change.
- Native audio is an option; turn it off for formats where the user adds a trend sound (cheaper).

## Wan (`wan_2_2_i2v_fast`)

- Cheap animation of an approved still. Lock the look as an image first, then animate.
- Prompt only motion, camera and one ambient detail. Keep it short and literal.
- If it cuts or morphs, add "single continuous shot" and cut to one action.
- Output is silent.

## Nano Banana stills (`nano_banana_pro`, `nano_banana_2`)

- Full sentences, specific colors, materials, light and position.
- Text: exact words in double quotes with style and position, plus "No other text".
- With a reference photo, number it and give it a role ("image 1 is the pet").
- A still that will become a first frame should already have the framing, light and look you want in the video.

## Animating a still (any model with a first frame)

- Prompt only what the image cannot show: motion, camera, timing, sound.
- Do not re-describe the image. Re-description makes faces and designs drift.
- Lock what must hold: "keep its design exactly as the first frame".
- Too still: add one physical action and one time cue. Hands break: simplify the hand motion.

## Motion transfer (`dreamactor_m2`)

- One image gives identity, one driving video gives motion.
- Best driving clips: one performer, one clear action, clean silhouette, steady camera.
- Frame the image like the video (both full body), plain background, one person.
- Transfers well: choreography and timing. Transfers poorly: fine hand detail and facial acting.
- Only motion should carry over, never the original performer's look.
