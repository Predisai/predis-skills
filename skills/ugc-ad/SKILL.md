---
name: ugc-ad
description: >-
  Makes one creator-style UGC video ad from a product brief: an AI creator holds the real product, says the hook in the first two seconds, and talks to camera in a phone-shot look. Default is one 20-second 9:16 ad with native speech, built from two shots and joined in order. Prices the job before spending.
  Use when: "/ugc-ad", "UGC ad", "UGC video ad", "creator-style ad", "TikTok-style ad for my product", "AI creator holding my product", "talking-to-camera product ad", "Use Predis to make a 20-second UGC video ad for".
  NOT for: a studio product shot with no person (product-video), a full ad set from a product link (link-to-ads), copying an ad the user already has (reference-ad), new hooks for an existing video (hook-variants), trend formats (viral-formats), image carousels (carousel), headline copy (headline-variants), fixing one frame (fix-frame), thumbnails (thumbnail).
---

# UGC video ad

One product brief in, one 20-second vertical ad out. An AI creator holds the real product and talks to camera like a phone video. The hook comes first.

Read [references/predis-mcp.md](references/predis-mcp.md) first and follow its loop for every generation. Prompt rules for the models used here are in [references/prompting.md](references/prompting.md).

## Rules

- **Real product photo.** The product in hand comes from a real photo of it. No photo: ask for one (a public image link works too). If the user only has a product page, use its main product image when it is a clean shot; otherwise ask.
- **No invented claims.** Benefits, prices and offers come from the user or their product page. If a fact is missing, leave it out.
- **The creator is not a customer.** The AI creator shows and explains. It never says "it changed my skin" or claims personal results. No before-and-after for health, beauty or fitness.
- **Speech only from native-audio video models.** No voiceover, TTS or lip-sync tool exists. Check `describe_model` lists an `audio` option before using a model for talking shots.
- **No captions in the video.** Prompts say "no subtitles". Offer the spoken lines as text so the user can add captions in their editor or app.
- **AI disclosure.** Tell the user the creator is AI-generated and to switch on the platform's AI label when they post, where the platform asks for it.

## Starting picks (check the live catalog)

| Job | Starting pick | Why |
|---|---|---|
| Creator still | `nano_banana_pro`, aspect `9:16` | Holds the product from a reference photo well |
| Talking shots | `seedance_2_0`, `references` mode, `duration` 10, `aspect` `9:16`, `resolution` `720p`, `audio` true | Up to 15 s per shot with native audio, takes the creator and the product as references |
| Best lip-sync, higher cost | `veo_3_1`, `keyframes` mode, creator still as first frame, shots of 8 + 6 + 6 s | Strongest speech; max 8 s per shot |
| Cheaper draft | `veo_3_1_fast`, same setup as `veo_3_1` | Same prompts, lower price |

No single model makes 20 s in one take, so the ad is always planned as shots. 720p keeps the phone look and the price down; offer 1080p if the user asks.

## Step 1. Brief (one message, only what is missing)

1. The product photo, the product name, and the one thing it does best.
2. Who buys it and the problem they have.
3. The call to action line (for example "Link in bio" or "Shop at brand.com").

A product link helps: read it for benefits and price. Do not ask about format or length. The default is 20 s, 9:16. Change it only if the user says so.

## Step 2. Plan the ad

Pick one format that fits the product:

| Format | Shot 1 (hook + problem or setup) | Shot 2 (payoff + CTA) |
|---|---|---|
| Problem-solution | Creator shows the annoyance, says the hook | Product fixes it on screen, CTA |
| Unboxing | Opens the box, lifts the product to camera, hook | One detail up close, verdict, CTA |
| 3 reasons | Holds product up: "Three reasons people switch from..." | Counts the reasons on fingers, CTA |
| Hack or tip | "Stop doing X. Do this instead." | Shows the hack with the product, CTA |
| Demo | Product in use in the first frame, hook | What it does, close-up, CTA |

Write the plan as two lines, one per shot: what happens, the exact spoken line, and seconds. Hook rules:

- The product is visible in the first frame.
- The hook line is said in the first 2 seconds and is under 8 words.
- Spoken lines fit the shot: about 2.5 words per second, so 20 to 24 words for a 10 s shot.
- The CTA is the last line of shot 2.

Write one casting line for the creator (age range, look, outfit, setting that fits the buyer). It goes into every prompt word for word.

## Step 3. Creator still

Add the product photo as a reference. Make 2 options with the creator still prompt in references/prompting.md, product as image 1. Look at both yourself first: the product must match the photo in shape, color and label, and be held in a real grip. Re-roll a bad one before showing.

Then show, in one message: the two creator options, the plan, and the total credits for the video shots from `estimate_cost`. Ask: "Pick a creator and I'll make the ad. Want to change a line first?"

## Step 4. Shots

Turn the chosen still into a reference with its asset id.

- Seedance: references are [creator still, product photo] = image 1, image 2. Two shots of 10 s each, sent in one batch.
- Veo: the creator still is the first frame of each shot. The product photo cannot be passed in this mode, so the still carries the product. Three shots of 8, 6 and 6 s.

Write each shot with the talking-shot template in references/prompting.md. Repeat the casting line in every shot so the creator stays the same.

## Step 5. Join the shots

Follow "Files and ffmpeg" in predis-mcp.md.

ffmpeg works: save shots as `shot-1.mp4`, `shot-2.mp4` in `./predis-output/ugc-ad/<product>/`, then join them with sound:

```
ffmpeg -i shot-1.mp4 -i shot-2.mp4 -filter_complex "[0:v][0:a][1:v][1:a]concat=n=2:v=1:a=1[v][a]" -map "[v]" -map "[a]" -c:v libx264 -crf 18 -c:a aac -movflags +faststart ugc-ad.mp4
```

For three Veo shots add `-i shot-3.mp4`, add `[2:v][2:a]` and set `n=3`.

No ffmpeg: share the shot links in order with exact cut notes, for example: "Shot 1 at 0:00, shot 2 at 0:10. Hard cuts, no transitions, keep each shot's own sound." Never say the shots were joined.

## Check before showing

Look at frames from each shot (or the first frame from the result card). Re-roll a shot once if any of these fail:

1. The product in hand differs from the photo (shape, color, label) or is held in a way no hand could.
2. The creator's face, hair or outfit changes between shots.
3. The product is not visible in the first frame, or the hook is not in shot 1.
4. On-screen text, subtitles, or a logo that is not the product's own.
5. It looks like a studio ad, not a phone video (perfect studio light on a selfie, glossy grade).

You cannot hear the audio. Ask the user to listen once for a clean voice and the right words.

## Deliver

One line: format, hook, length, credits spent. Then the spoken lines as plain text (for captions), and the AI label reminder. Offer one next step: re-roll one shot, or a second ad in another format with the same creator.

## Edge cases

- **Apparel:** the creator wears the item: "wearing the garment from image 2".
- **Rushed or cut-off speech:** shorten the line and re-roll that shot.
- **User wants their own face:** use their photo as the creator still, only with their consent.
- **Longer or shorter ad:** re-plan the shots within each model's `duration` values. A 10 s ad is one Seedance shot.
