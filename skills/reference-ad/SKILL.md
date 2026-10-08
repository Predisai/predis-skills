---
name: reference-ad
description: >-
  Remakes an ad the user likes for their own product through the Predis connector. Works with a static
  image ad or a video ad. Maps the reference's structure (layout, text zones, beats, pacing, hook type),
  rewrites the copy for the user's product, gets one OK, then makes the new ad with the user's real product.
  Keeps the structure, drops the other brand's logo, copy and people. Use when: "/reference-ad", "remake
  this ad for my product", "make this ad but for us", "copy this ad's layout", "same structure as this ad",
  "I like this ad, do one for my brand". NOT for: ads from a product page link (link-to-ads), UGC creator
  videos from scratch (ugc-ad), a product video from scratch (product-video), a swipe carousel (carousel),
  new hooks on an existing video (hook-variants), new headlines on an existing ad (headline-variants),
  replacing one shot in a video ad (fix-frame), a thumbnail (thumbnail), trending social formats (viral-formats).
---

# Remake a reference ad

Share an ad you like, get the same structure for your product. Keep the skeleton. Swap everything that belongs to the other brand.

First, read [references/predis-mcp.md](references/predis-mcp.md) and follow its loop for every generation. Model prompting rules are in [references/prompting.md](references/prompting.md).

## Rules

- Same structure, not "improved": same zones, hierarchy, rhythm and hook type.
- Never carry over the other brand's logo, name, product, copy, claims or people. Cast a new person with a similar vibe.
- The user's real product only. No product photo, no remake: ask for one.
- Facts (brand name, claims, price, offer) come from the user. Missing? Ask, or drop that zone and say so.
- The reference ad is not sent to the model by default. Your written breakdown carries the structure.

## Step 1: Get the inputs

- The reference ad: an image, a video, or a link to one.
- A product photo: a public link, an attached file, or a file already in Predis (see predis-mcp.md).
- One line on what the product is and does. Brand name, logo and any claim or offer only if a zone needs them.

Image means static mode. Video means video mode. A video where the user wants a still means static mode on its best frame.

## Static mode

**S1. Break it down.** Look at the reference and note:
- Aspect ratio and look (studio packshot, lifestyle photo, phone photo, flat graphic, before/after, review card).
- Zones from top to bottom: position, rough share of the frame, what is in it (background, product, headline, subhead, badge, CTA, logo, legal).
- Hierarchy: what the eye hits first, second, third.
- Palette, light, product angle and size, text style per zone (weight, case, color, alignment).
- One formula line: "pain headline top center, product centered on cream with a prop, badge top right, CTA pill bottom."

Show the formula and the zone list.

**S2. Rewrite the copy.** Zone by zone, same length (within about 20%) and tone. Swap only the brand-specific parts. Drop legal lines unless the user gives their own.

```
Headline: "Your skin, but rested."  ->  "Your hair, but rested."
Badge:    "NEW"                     ->  "NEW"
CTA:      "Shop the serum"          ->  "Shop the mask"
2 takes, about 14 credits. Good to go, or change a line?
```

Wait for the OK.

**S3. Make it.** Starting pick: `nano_banana_pro`, aspect matched to the reference (it lists 1:1, 4:5, 16:9, 9:16). If the ad carries more than about 12 words or 3 or more text zones, use `gpt_image_2` with the closest `output_size`. Product photo is image 1, logo image 2. Make 2 takes in one batch.

**S4. Fix text.** If one zone is wrong on the best take, run one focused edit on that take: get its asset id with `check_run`, pass it with `add_reference(asset_id=...)` as image 1 (see prompting.md). Do not keep re-rolling for text.

**S5. Stronger option.** Only if both takes miss the structure, offer once: "I can show the model the original ad as a layout guide. The result gets closer to their creative, so it is more derivative, and that risk is yours. Want that?" On a clear yes only, add it as the last reference and say in the prompt it is a layout guide only, with every logo, name, product, person and word removed.

## Video mode

**V1. See it.** If you can run ffmpeg (see predis-mcp.md), pull a frame every 0.5 s from the first 3 seconds and one per scene cut, and look at them. If you cannot, ask the user for 3 to 6 screenshots in order, or a short beat list. You cannot hear the video: get spoken words from on-screen captions, the ad text, or the user. Never guess them.

**V2. Break it down.** Note:
- Aspect, total length, where the hook ends, number of cuts and rough shot length.
- Hook type: problem shown, question to camera, bold claim, before/after, unboxing, demo, reaction.
- Beat list, one line per shot: framing, action, camera move, what the product does.
- Who is on screen (type, energy, setting), not who they are.
- Script, marked spoken, voice-over or none. Text on screen, word for word.
- One formula line: "everyday failure shown, one-line complaint to camera, product enters at 2 s, quick demo, CTA."

Show the formula, beats and script.

**V3. Adapt.** Rewrite the script for the product: same length and turn, about 2.5 words per second. On-screen text and captions are not made by the model; list them as edits the user adds in their editor. Ask: "Good to go, or change a line?"

**V4. Make it.** Starting picks:
- Someone speaking to camera: `veo_3_1` (native speech, 9:16 or 16:9, up to 8 s). There is no voiceover or lip-sync tool.
- No speech, or several shots: `seedance_2_0` (labeled shots in one clip, 4 to 15 s, 9:16, 1:1, 16:9 and more).

Product photo as image 1. Match the reference aspect and the shortest duration that covers the beats. Price it, state it, then make 1 take. Offer a second take after the user sees the first.

## Check before showing

Look at every image. For video, look at one frame if you can extract it. Hard fails:
1. Any trace of the other brand: logo, name, product, copy, or the same person.
2. Product shape, label or color differs from the user's photo.
3. On-image text differs from the approved copy by even one letter.
4. The structure is lost: zones or beats not where the reference had them.
5. Video: frozen motion, warped product, or stray text and logos in frame.

One failed take gets one re-run with a new key. Fails again: show the best one and say what is off in one line.

## Deliver

```
Your version of the reference, for Acme's hair mask.
Kept: headline top, product center on cream, badge top right, CTA pill bottom.
Changed: product, copy, colors to yours. Their logo and copy removed.
Used 14 credits.
```

For video, add the edits for the user's editor: captions and text on screen with timings. Offer one next step: another take, or the other aspect.

## Edge cases

- Reference is a screenshot of an app or site: ask for the user's real screen. Never invent UI.
- Split or before/after layout: map each panel as its own zones and say which panel gets the product.
- User only wants the breakdown: stop after S1 or V2. No credits spent.
