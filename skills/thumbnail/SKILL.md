---
name: thumbnail
description: >-
  Makes truthful, click-worthy thumbnails and covers with the Predis connector. YouTube 16:9 thumbnails, Shorts and Reels 9:16 covers, and 4:5 feed covers. Offers three curiosity concepts, locks the user's face or logo as a reference, renders 1 to 4 words of bold text, makes three variants, does one focused edit, and checks that text and face read at phone size. Use when: "/thumbnail", "make a thumbnail", "YouTube thumbnail for this video", "Shorts cover", "Reels cover", "cover image for my video", "thumbnail with my face", "A/B thumbnail options". NOT for: the video itself (viral-formats or product-video), ads (link-to-ads, ugc-ad, reference-ad), multi-slide posts (carousel), fixing one bad frame of a video (fix-frame), or text-only hook and headline lists (hook-variants, headline-variants).
---

# Thumbnail

Three honest concepts, one pick, three strong variants, checked at the size people actually see them.

First, read `references/predis-mcp.md` and follow its loop for every Predis call. Prompt rules for the models here are in `references/prompting.md`. Concept method is in `references/concepts.md`.

## 1. Intake (ask only what is missing, in one message)

- The video: title or topic, and what really happens in it. This is the truth the thumbnail may promise.
- Format: 16:9 YouTube (default), 9:16 Shorts or Reels cover, or 4:5 feed cover.
- Who appears: the user (1 to 3 clear face photos), another person who agreed, a generated person, or nobody. Never put a real person in silently.
- A logo or product to show? (file or link, optional)
- A thumbnail they like the look of? Read it for energy, palette and layout only. Never pass it as a reference and never copy its people or exact layout.
- Text: yes by default, 1 to 4 words.

## 2. Three concepts (no credits)

Follow `references/concepts.md`. Brainstorm at least 6, keep the best 3, show them as three short lines:

> 1. **Shocked face + melted phone** (big face) · text: `IT MELTED`
> 2. **Before / after desk** (before/after) · text: `30 DAYS`
> 3. **Tiny me, giant phone** (size contrast) · no text

Wait for a pick. Mixing is fine ("2 with the face from 1").

## 3. References

Turn each face photo and the logo into a reference with the steps in `predis-mcp.md`. Order: faces first, then the logo. The prompt calls them "image 1", "image 2" in that same order.

## 4. Pick the model (starting picks)

The catalog changes. Confirm each pick with `list_models` and `describe_model`.

| Case | Starting pick | Size setting |
|---|---|---|
| Text in the image (default) | `gpt_image_2` | `output_size` 1920x1080 or 1080x1920, `quality` high |
| Face likeness first, little or no text | `nano_banana_pro` | `aspect` 16:9, 9:16 or 4:5, `resolution` 2K |
| 4:5 feed cover | `nano_banana_pro` | `aspect` 4:5 |
| Premium text-heavy | `gpt_image_2_5_flare` | same sizes as `gpt_image_2` |
| Cheap drafts | `nano_banana_2` | `aspect` as above |

If the live model lacks the ratio you need, use the closest size it offers and say the user should crop, or crop with ffmpeg per `predis-mcp.md`.

## 5. Write the prompt

In this order:

1. **References** (only if any): `IMAGE REFERENCES: image 1 = CHARACTER 1 face; image 2 = brand logo.`
2. **Frame:** `Bold, high-impact video thumbnail, <ratio>, one continuous scene, bright and punchy, poster-grade.` For 9:16 add `faces and text in the upper two-thirds`.
3. **Scene:** the picked concept, concrete and true to the video.
4. **Subject:** `large in the foreground, chest-up, about half the frame, face sharp.` Per referenced person: `CHARACTER 1 is the exact person from image 1: same face shape, eyes, nose, lips, jawline, skin tone and hair. Do not beautify or restyle the face. Expression: <one emotion, described physically>.`
5. **Key element:** only the one prop or effect that creates the gap.
6. **Logo** (if any): `the logo from image N, exact shapes, colors and letters, away from the face.`
7. **Text:** `Headline text reading exactly "<TEXT>" in a huge heavy sans-serif, <color> with a thick black outline, placed <top / left side>, never over the face, nothing in the bottom-right corner. No other text, no watermark.` No text wanted: `No text, no letters, no watermark.`
8. **Light:** `strong key light on the face, bright rim light separating subject from background, vivid saturated color, deep blacks, simple background with a soft vignette.`

## 6. Three variants

Estimate one image, times three, and say the total in one line. Send three `generate` calls in one batch with the same references and settings and a fresh key each. Change exactly one line per variant (expression, camera distance, or text color) so they are real choices.

## 7. Check before showing (no credits)

Look at every variant yourself. If ffmpeg works, also downscale each to 320 px wide (`scale=320:-1`, or `scale=-1:320` for 9:16) and look at the small copies. Without ffmpeg, judge as if the image were the size of a phone thumbnail.

Hard fails. Any one means retry once with a fresh key, or fix it in the edit pass:

- Text unreadable at 320 px, or more than about 5 words.
- Any word misspelled or different from the chosen text.
- Face broken at small size (eyes, teeth, hands), or it does not match the reference photo.
- Something important in the bottom-right corner (16:9) or the bottom third (9:16).
- A promise the video does not keep, or a logo, person or brand that is not the user's.
- Wrong aspect ratio for the chosen format.

Show the passing variants with a short label each ("shock, close", "shock, wider", "yellow text"). The user picks one.

## 8. One focused edit (optional)

One change at a time on the pick. Make the picked output a reference (`add_reference` with its asset id), then edit with `nano_banana_pro`, or `gpt_image_2` when the change is text, passing it as image 1:

```text
Edit image 1. Change ONLY <the text to read exactly "NEW TEXT" | the expression to <emotion> | the background to <new background>>. Keep everything else exactly the same: face, pose, clothing, logo, composition, light, colors.
```

State the credits for each edit. Re-run the check after every edit.

## 9. Deliver

Share the final link. If ffmpeg works, also export a sized JPEG into `./predis-output/thumbnail/<job>/`:

```bash
ffmpeg -y -loglevel error -i pick.png -vf "scale=1280:720:force_original_aspect_ratio=increase,crop=1280:720" -q:v 3 thumbnail-1280x720.jpg
```

9:16 uses `1080:1920`, 4:5 uses `1080:1350`. Then say the format and credits spent, and offer the other two variants for an A/B test or the same concept in another format.

## Guardrails

- Truthful: no invented results, numbers, people, quotes or product claims.
- Faces only of the user or people who agreed. No celebrities or public figures.
- No platform, network or other brands' logos, only the user's own.
- State credits before every generation. Ask first if the job is over 150 credits.
