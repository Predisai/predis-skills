---
name: product-video
description: >-
  Makes a studio-style video of one product from a real product photo. Default is one 8-second 9:16 shot: the photo becomes a clean studio still, then one smooth camera move brings it to life, with the product kept exactly as photographed. Other aspect ratios on request. Prices it before spending.
  Use when: "/product-video", "product video", "studio product video", "animate my product photo", "product shot video", "hero video of my product", "8-second product clip", "Use Predis to make an 8-second studio product video".
  NOT for: a creator talking about the product (ugc-ad), a full ad set from a product link (link-to-ads), copying an ad the user already has (reference-ad), trend formats (viral-formats), image carousels (carousel), new hooks for an existing video (hook-variants), headline copy (headline-variants), fixing one frame (fix-frame), thumbnails (thumbnail).
---

# Product video

One real product photo in, one studio-style product shot out. The product keeps its exact shape, label and colors.

Read [references/predis-mcp.md](references/predis-mcp.md) first and follow its loop for every generation. Prompt rules for the models used here are in [references/prompting.md](references/prompting.md).

## Rules

- **Real product photo.** The video starts from a real photo of the product. No photo: ask for one (a public image link works too). If the user gives a product page, use its main product image only if it is a clean shot.
- **One shot.** Default is a single 8 s shot. Plan more shots only when the user asks for a longer video.
- **No people unless asked.** Hands or a person only when the user wants the product in use.
- **No text in the video.** No titles, prices or logos are generated into the shot. The product's own label is the only text.
- **Sound.** Only effects tied to the product (a click, a pour, a soft whoosh), never music or voice. If the user does not want sound, set `audio` to false.

## Starting picks (check the live catalog)

| Job | Starting pick | Why |
|---|---|---|
| Studio still | `nano_banana_pro`, `aspect` to match the video | Keeps the product faithful from one reference |
| Motion, default | `kling_3_standard`, `keyframes` mode, still as `first_frame`, `duration` 8, `aspect` `9:16` | Faithful image-to-video, 5 to 10 s, 9:16, 16:9 and 1:1 |
| Sound sells it (pour, fizz, click) | `veo_3_1`, `keyframes` mode, `duration` 8 | Best native sound; 9:16 and 16:9 only |
| 4:3 or 3:4 | `seedance_2_0`, `keyframes` mode | Lists those aspects |

## Aspect ratios

| Asked for | Still | Video |
|---|---|---|
| 9:16 (default), 16:9, 1:1 | same aspect | `kling_3_standard`, same aspect |
| 4:3 or 3:4 | closest still aspect the image model lists | `seedance_2_0`, asked aspect |
| 4:5 | 4:5 | closest video aspect (3:4 on `seedance_2_0`), then crop to 4:5 |

Check `describe_model` for the current aspect lists before promising one. The 4:5 crop follows "Files and ffmpeg" in predis-mcp.md:

```
ffmpeg -i product-video.mp4 -vf "crop=iw:iw*5/4,scale=1080:1350" -c:v libx264 -crf 18 -c:a copy product-video-4x5.mp4
```

No ffmpeg: share the 3:4 video and tell the user to crop it to 4:5 in their editor or in the app when posting, keeping the product centered.

## Step 1. Brief (one message, only what is missing)

1. The product photo and the product name.
2. A look, if they have one in mind: dark and premium, bright and clean, warm, or bold color. Default is a clean, soft-lit studio look on a seamless backdrop.

Do not ask about length or format. Default is 8 s, 9:16.

## Step 2. Studio still

Add the product photo as a reference. Write the still prompt with the template in references/prompting.md, product as image 1, at the video's aspect.

Look at the still before showing it:
- Shape, label text and colors match the photo.
- The product is sharp, whole, and not touching the frame edge.
- No extra text or logos.

Re-roll a bad still once. It is far cheaper than a bad clip.

Then show the still with one line on the planned camera move and the clip price from `estimate_cost`. Ask: "Animate this?"

## Step 3. Motion

Turn the approved still into a reference with its asset id. Pass it as the first frame. Write the motion prompt with the template in references/prompting.md: camera move first, then what moves, then the product lock.

Pick one camera move that fits the look:

| Look | Camera move | Light change |
|---|---|---|
| Dark, premium | Slow push-in | A light sweep crosses the product |
| Bright, clean | Slow 90 degree orbit | None, soft and even |
| Warm | Gentle drift | Soft sunlight shifts across the surface |
| Bold color | Slow orbit or crane up | Color rim light pulses once |

## Longer video (only when asked)

Plan 2 or 3 shots (hero reveal, close-up detail, final hero hold), each from its own still made with the first still as a style reference. Price the whole job first. Join the shots as predis-mcp.md says:

```
ffmpeg -i shot-1.mp4 -i shot-2.mp4 -filter_complex "[0:v][0:a][1:v][1:a]concat=n=2:v=1:a=1[v][a]" -map "[v]" -map "[a]" -c:v libx264 -crf 18 -c:a aac -movflags +faststart product-video.mp4
```

With `audio` false on all shots, drop the audio parts: `[0:v][1:v]concat=n=2:v=1:a=0[v]` and map only `[v]`. No ffmpeg: share the shots in order with exact cut points.

## Check before showing

Look at the first and a late frame of the clip. Re-roll once with a fix from references/prompting.md if any of these fail:

1. The product changes shape, label or color during the clip.
2. Text, captions or logos appear that are not on the real product.
3. The camera move is missing or the shot is frozen.
4. The surface flickers or the texture boils on the product.
5. The wrong aspect ratio.

## Deliver

One line: length, aspect, the camera move, credits spent. Offer one next step: the same shot in another aspect ratio, or a different camera move.
