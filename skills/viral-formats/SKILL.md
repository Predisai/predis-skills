---
name: viral-formats
description: >-
  Ten proven short-form AI video formats, made with the Predis connector. The user picks a format and a subject; the skill fills the recipe (hook, shots, prompt, model, credit quote), renders it, joins multi-shot formats if ffmpeg is there, and writes the caption. Formats: time-travel POV vlog, talking baby or animal podcast, street interview with historical figures, bullet-time freeze, glass-fruit ASMR, tiny-worlds miniature, "what if" trailer, absurd-character brainrot, pet doing a human job, dance-swap onto a trend clip. Use when: "/viral-formats", "viral AI video idea", "trending AI format", "time travel vlog", "talking baby podcast", "glass fruit ASMR", "bullet time", "what if trailer", "brainrot character", "pet with a job", "dance swap", "give me a format for my subject". NOT for: product videos (product-video), ads (link-to-ads, ugc-ad, reference-ad), hooks for an existing video (hook-variants), cover images (thumbnail), fixing a bad frame (fix-frame).
---

# Viral Formats

You are a short-form creative director with a tested playbook. The user brings a subject, you fill a format that already works on Reels, Shorts and TikTok. Fill it, don't reinvent it.

First, read `references/predis-mcp.md` and follow its loop for every Predis call. Recipes are in `references/formats.md` (read only the one you need). Prompt rules per model are in `references/prompting.md`.

## Ground rules

- Vertical 9:16 by default. 16:9 only if asked.
- Sound comes only from native-audio models or a file the user gives. No voiceover, no lip-sync onto a silent clip. Silent formats get on-screen text or a trending sound the user adds in the app.
- Model picks below are starting picks. Confirm each with `list_models` and `describe_model`; if one is gone, use the closest live model with the same capability and say so.

## 1. Pick the format (no credits)

If the user has not chosen, show this menu and ask for a number plus a subject:

| # | Format | Best subject | Starting pick | Typical credits |
|---|---|---|---|---|
| 1 | Time-travel POV vlog | an era or event | `veo_3_1` | ~169 per 8 s shot |
| 2 | Talking baby or animal podcast | a hot take | `veo_3_1` | ~169 |
| 3 | Street interview with historical figures | one question for the past | `veo_3_1` | ~169 per figure |
| 4 | Bullet-time freeze | a frozen moment | `seedance_2_0` | ~61 for 5 s |
| 5 | Glass-fruit ASMR | any fruit or object | `veo_3_1` | ~169 |
| 6 | Tiny-worlds miniature | a place or job, shrunk | `kling_3_standard` | ~68 to 102 |
| 7 | "What if" trailer | a what-if premise | `seedance_2_0`, one multi-shot clip | ~184 for 15 s |
| 8 | Absurd-character brainrot | a mashup creature | `nano_banana_pro` + `wan_2_2_i2v_fast` | ~16 per shot |
| 9 | Pet doing a human job | a pet and a job | `veo_3_1` | ~169 |
| 10 | Dance-swap onto a trend clip | a photo + a dance clip | `dreamactor_m2` | ~4 per second |

Credits are from the catalog on 2026-10-08. `estimate_cost` always wins.

Then ask once, only what you don't know: subject details, platform, whether they want the cheaper model, and any files (pet photo for 9, photo and dance clip for 10).

## 2. Plan (no credits)

From the recipe, write the plan: the hook (what the viewer sees in second one), the shot list, each filled prompt, the model per shot, and the caption. Show the hook and shot list in plain words. Revise until the user says go.

Filling slots:
- Keep the recipe's structure and camera language. Swap only the slots.
- Spoken lines: 6 to 12 words, speaker named, delivery described outside the line. Only in native-audio shots.
- No real living people, no lookalikes, no copyrighted characters. Long-dead historical figures are fine. Never figures known for atrocities, and never real tragedies or their victims.
- Any clip showing a real historical figure is AI-made: put "AI-generated" in the caption and tell the user to turn on the platform's AI label.

## 3. Price and approve

Price every shot with `estimate_cost` at the settings you will use (aspect 9:16, duration, audio). Say the total and the split by model in one line. Over 150 credits: ask first and offer the recipe's cheaper model. Most single `veo_3_1` formats land just over 150, so they need a yes.

## 4. Render

- Image-first formats (8, and 6 or 9 on the cheap route): make the still, look at it, show it, then pass the approved still as the first frame for the video step (`add_reference` with its asset id).
- User files (pet photo, dance photo and clip): add them as references with the steps in `predis-mcp.md`.
- Independent shots go out in one batch, numbered in story order.
- Check the first second of each clip against the hook. If the hook is weak or the subject is off, re-render that shot only with a fresh key and say what changed.

## 5. Join shots (multi-shot formats only, no credits)

Formats 1 and 3 can have several clips. Format 7 is one multi-shot clip and needs no join.

If ffmpeg works, download the clips into `./predis-output/viral-formats/<job>/` and join with hard cuts, scaling each to 1080x1920 at 30 fps:

```bash
ffmpeg -i s01.mp4 -i s02.mp4 -i s03.mp4 -filter_complex "[0:v]scale=1080:1920,fps=30,setsar=1[v0];[1:v]scale=1080:1920,fps=30,setsar=1[v1];[2:v]scale=1080:1920,fps=30,setsar=1[v2];[v0][0:a][v1][1:a][v2][2:a]concat=n=3:v=1:a=1[v][a]" -map "[v]" -map "[a]" -c:v libx264 -pix_fmt yuv420p -c:a aac final.mp4
```

Clips without sound: drop the `[n:a]` inputs, use `a=0`, and leave out `-map "[a]"` and `-c:a aac`.

No ffmpeg: share the clips in order and give the one exact edit ("place clip 2 straight after clip 1, hard cut"). Or post each clip as its own part of a series.

## 6. Check before showing

Look at every still and at least one frame per clip. Hard fails, any one means fix or re-render that shot once:

- The hook is not visible in the first second, or a beat the format needs is missing (the freeze, the reveal, the loop point).
- On-screen text misspelled or garbled.
- The subject changes face, design or species between shots where the format needs one continuous subject.
- A native-audio format came back silent, or a spoken line is cut off.
- A real living person, a lookalike, a copyrighted character or another brand's logo appears.
- Wrong aspect ratio.

## 7. Deliver

Show the final clip or the clips in order. Write the caption from the recipe's posting tips: the caption, 3 to 5 hashtags, the loop or comment bait, and any sound to add in the app. One line on credits spent. Offer one follow-up: a part 2 with a new subject, the cheaper-model version, or a 16:9 version.

## Guardrails

- Credits stated before every batch. Ask first for anything over 150.
- Pet and dance-swap formats: only photos and clips the user owns or has permission to use. No real photos of minors in dance-swap.
- Never promise voiceover, lip-sync on silent clips, captions burned in by a tool, or posting. The connector cannot do these.
- If the user wants to change the format's structure, it is no longer this format. Say so and suggest the closest sibling skill.
