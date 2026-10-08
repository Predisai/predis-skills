# Prompting for product videos

Rules for the models this skill uses. Option names and values always come from `describe_model`.

## Studio still (Nano Banana Pro)

- Give the reference a numbered role: image 1 is the product.
- A clean product photo works best: plain background, label facing camera.
- Name the light and the surface. Skip vague words like "stunning" or "8K".

Template:

```
Studio product photograph of the product from image 1. Keep it exactly as in image 1: same shape, label, colors and proportions. It stands [PLACEMENT] on [SURFACE] against a seamless [BACKDROP COLOR] backdrop. [KEY LIGHT], [RIM OR FILL LIGHT], soft natural shadow under the product. Three-quarter front angle, product centered with space around it, sharp focus on the label. [ASPECT] frame. No text, no extra logos, no props unless named.
```

Light by look:
- Dark, premium: low-key, hard rim light from behind, deep charcoal backdrop.
- Bright, clean: high-key softbox from above, white or pale backdrop.
- Warm: low golden side light, cream or wood surface.
- Bold color: saturated solid backdrop, crisp key light, colored rim.

## Motion (Kling 3 Standard, Veo 3.1, Seedance 2.0)

The still is the first frame. Describe only what changes: the camera, any motion, the light, the sound. Re-describing the product makes it drift.

Template:

```
[ONE CAMERA MOVE WITH A START AND END] over [N] seconds, steady and smooth. [ENVIRONMENT MOTION, IF ANY]. [LIGHT CHANGE, IF ANY]. The product stays rigid and keeps its exact shape, label and colors throughout. No morphing, no text appearing. Single continuous shot, no cuts. Sound: [ONE PRODUCT SOUND], no music, no voice.
```

Camera moves that hold up:
- "Slow push-in from a medium shot to a close-up of the label"
- "Smooth 90 degree orbit around the product, product stays centered"
- "Slow crane up from the surface to a slightly high angle"
- "Macro glide from left to right across the surface, shallow focus"
- "Locked-off camera, only the light moves"

Model notes:
- Kling: one camera move per shot, written with where it starts and ends. Cause and effect beats a list of small moves ("the light sweep reaches the label and it gleams").
- Veo: strongest for sound. Name the effect tied to an action ("cap clicks shut") and write "no music". Without a named sound it invents one.
- Seedance: keep it to 40 to 110 words. Use "single continuous take, no cuts" or it may cut on its own.

## Fixes

| Problem | Fix |
|---|---|
| Product morphs or label melts | Simpler camera move, add "product rigid and unchanged", shorter duration |
| Camera move ignored | Put the move first, one move only |
| Camera wanders | Name the start and end of the move, or "locked-off camera" |
| Text appears | Remove any quoted words from the prompt, keep "no text appearing" |
| Too still | Add one visible change: a light sweep, steam, drifting particles |
| Surface flickers | Slower move, plainer backdrop |
