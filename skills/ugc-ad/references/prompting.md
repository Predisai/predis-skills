# Prompting for UGC ads

Rules for the three model families this skill uses. Option names and values always come from `describe_model`.

## Creator still (Nano Banana Pro)

- Give every reference a numbered role in the prompt. Here image 1 is the product.
- Keep the product photo clean: plain background, label facing camera.
- Describe a real phone photo, not a fashion shoot.

Template:

```
Vertical 9:16 smartphone photo. [CASTING LINE]. They hold the product from image 1 at chest height, label toward the camera, exactly as in image 1: same shape, colors and label. Relaxed, friendly expression, looking into the lens. Soft window light, natural skin texture, ordinary home background. No text, no logos other than the product's own label.
```

## Talking shot (Seedance 2.0)

- Write it as: subject, action, scene, camera, light, sound, locks. Keep it to about 40 to 110 words.
- In references mode give each reference one job: image 1 sets the creator's identity, image 2 sets the product. Do not re-describe what the images show.
- Dialogue goes in quotes, in the shot where the speaker is on screen. Keep the face large and the camera calm for clean lips.
- Name the sounds of the room, or the model invents them.
- Swap vague words ("cinematic", "dynamic", "epic") for the actual shot size, move and light.

Template:

```
The creator from image 1, same face, hair and outfit, holds the product from image 2, exactly as shown: same shape, colors and label. [ONE ACTION WITH AN END POINT]. [SETTING]. Medium close-up, handheld phone at eye level, small natural sway. Soft window light. The creator says, [DELIVERY NOTE], "[LINE]". Sound: [ROOM TONE], no music. No subtitles, no on-screen text, no extra logos. Vertical 9:16.
```

## Talking shot (Veo 3.1 and Veo 3.1 Fast)

- The creator still is the first frame. Describe only motion, camera and sound. The look carries over from the image.
- Write dialogue after a colon, not in quotes: `She says: Okay, look at this lid.` Then add `(no subtitles)`.
- About 2 to 3 short sentences per 8 seconds. Too many words get rushed or cut.
- Prompt all four sound layers: dialogue, room sound, effects tied to actions, and music (say "no music").
- Draft on Fast, finish on Veo 3.1 with the same prompt.

Template:

```
The creator keeps the product in hand, label toward camera, product unchanged. [ONE ACTION]. Handheld phone, eye level, slight sway, face large in frame. They say, [DELIVERY NOTE]: [LINE]. Sound: [ROOM TONE], [ACTION SOUND], no music. (no subtitles)
```

## Fixes

| Problem | Fix |
|---|---|
| Creator's face drifts | Cut description of the face, keep "same face, hair and outfit as image 1" |
| Product warps in hand | Simpler hand action, add "the product keeps its exact shape and label" |
| Lips out of sync | Shorter line, tighter framing, no head turn during the line |
| Subtitles appear | Repeat "No subtitles" and remove any other quoted words |
| Laugh track or music appears | Name the room sound and write "no music" |
