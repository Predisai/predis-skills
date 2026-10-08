---
name: hook-variants
description: >-
  Keeps the user's video ad and swaps only the first 3 seconds, 5 different ways. Writes 5 hook concepts
  that each use a different hook type, gets one OK, generates 5 new openings that match the ad's look, and
  splices each onto the rest of the original ad. Outputs are named A to E so they can be split tested. With
  no ffmpeg, it delivers the 5 hook clips with exact splice steps for any editor. Use when: "/hook-variants",
  "make 5 new hooks for this video ad", "swap the first 3 seconds", "new openings for my ad", "test hooks on
  this video", "my ad has a weak hook". NOT for: new ads from a product link (link-to-ads), UGC creator videos
  (ugc-ad), a new product video (product-video), remaking an ad you like (reference-ad), a swipe carousel
  (carousel), new headlines on a static ad (headline-variants), changing one shot in the middle of a video
  (fix-frame), a video thumbnail (thumbnail), trending social formats (viral-formats).
---

# Hook variants

The body of the ad stays exactly as it is. Only 0:00 to 0:03 changes. 5 versions, A to E, each with a different kind of hook, so a split test tells the user which opening stops the scroll.

First, read [references/predis-mcp.md](references/predis-mcp.md) and follow its loop for every generation. Model prompting rules are in [references/prompting.md](references/prompting.md).

## Step 1: Get the ad

Ask for the video ad if it is not in the chat. Also ask, in the same message, only what you cannot see:
- What the product is and who the ad is for, if the ad does not make it clear.
- Any line or claim that must not appear (optional).

Watch the ad. If you can run ffmpeg, probe it first:

```bash
ffprobe -v error -select_streams v:0 -show_entries stream=width,height,r_frame_rate -show_entries format=duration -of default=nw=1 ad.mp4
ffprobe -v error -select_streams a -show_entries stream=index -of csv=p=0 ad.mp4
```

Note width, height, fps, length, and whether it has audio. Then pull two stills: the look (just after the splice) and the old opening.

```bash
ffmpeg -y -ss 3.5 -i ad.mp4 -frames:v 1 -q:v 2 look.jpg
ffmpeg -y -ss 0.5 -i ad.mp4 -frames:v 1 -q:v 2 old_hook.jpg
```

No ffmpeg: ask the user for one screenshot from just after 0:03. If they can't, use the ad itself as the reference when it is 15 seconds or shorter.

Write down what you see after 0:03: setting, light, grade, camera style (phone or studio), people, product, pace. The new hooks must lead into that.

## Step 2: Write 5 hook concepts

Each concept uses a different hook type. Pick 5 of these, whichever fit the ad:

| Type | What happens in second 0 to 1 |
|---|---|
| Question | Someone mid-problem, the moment the buyer would search for help |
| Bold claim | The product doing its most checkable thing, up close |
| Callout | A person turns to camera, the "you're doing it wrong" moment |
| Before/after | The bad state and the good state in one beat |
| POV | A very specific, relatable first-person moment |
| Proof first | Open on the end result |
| Pattern break | Something visually odd with the product |

Rules:
- Each concept is one visual beat with an endpoint, written in one or two plain lines.
- Every claim comes from the ad or the user. No invented numbers, reviews or offers.
- Each hook must hand off cleanly to the shot at 0:03. Same world, same product, same kind of camera.
- No on-screen text in the clip. Models garble it.

Show them as a short table: letter, hook type, what the viewer sees. Then the cost (Step 3) and one question: "Good to go, or want to swap any?"

## Step 3: Price and get one OK

Starting pick: `seedance_2_0_fast`, 4 seconds (its minimum), audio off, aspect closest to the ad (`9:16`, `16:9`, `1:1`, `3:4` or `4:3`; a 4:5 ad uses `3:4` and is cropped in the splice). The extra second is trimmed off. Upgrade pick for a final: `seedance_2_0`. Check both with `describe_model` first.

Run `estimate_cost` once and multiply by 5. Tell the user the total in the same message as the concepts. One OK covers the whole job.

## Step 4: Generate 5 hooks

- Reference: the look still as image 1 (or the ad as video 1 if that is all you have).
- One prompt per concept, written with [references/prompting.md](references/prompting.md).
- Five takes in one batch, each with its own idempotency key.
- Look at a frame of every result. Re-roll a take once if it fails a check below.

## Step 5: Splice

Default audio: keep the ad's own soundtrack from 0:00 to the end, so music and voiceover stay in place. If the old opening has speech that belongs to the old hook, ask the user once: keep it, or use new sound per hook (then generate with audio on).

**With ffmpeg.** Download the hooks into `./predis-output/hook-variants/<ad-name>/`. For each letter, swap in the ad's W, H and FPS from Step 1:

```bash
ffmpeg -y -i hook_B.mp4 -i ad.mp4 -filter_complex \
 "[0:v]trim=0:3,setpts=PTS-STARTPTS,scale=W:H:force_original_aspect_ratio=increase,crop=W:H,fps=FPS,setsar=1,format=yuv420p[h]; \
  [1:v]trim=start=3,setpts=PTS-STARTPTS,fps=FPS,setsar=1,format=yuv420p[b]; \
  [h][b]concat=n=2:v=1:a=0[v]" \
 -map "[v]" -map 1:a? -c:v libx264 -crf 18 -preset medium -c:a aac -b:a 192k -shortest <ad-name>_hook_B.mp4
```

With new sound per hook (both files must have audio):

```bash
ffmpeg -y -i hook_B.mp4 -i ad.mp4 -filter_complex \
 "[0:v]trim=0:3,setpts=PTS-STARTPTS,scale=W:H:force_original_aspect_ratio=increase,crop=W:H,fps=FPS,setsar=1,format=yuv420p[h]; \
  [0:a]atrim=0:3,asetpts=PTS-STARTPTS,aresample=48000,aformat=channel_layouts=stereo[ha]; \
  [1:v]trim=start=3,setpts=PTS-STARTPTS,fps=FPS,setsar=1,format=yuv420p[b]; \
  [1:a]atrim=start=3,asetpts=PTS-STARTPTS,aresample=48000,aformat=channel_layouts=stereo[ba]; \
  [h][ha][b][ba]concat=n=2:v=1:a=1[v][a]" \
 -map "[v]" -map "[a]" -c:v libx264 -crf 18 -preset medium -c:a aac -b:a 192k <ad-name>_hook_B.mp4
```

Check each output with ffprobe: same width, height, fps and length (within 0.1s) as the original.

**Without ffmpeg.** Share the 5 hook links and give the same edit for each, with the letter filled in:
> Put hook B at 0:00 on its own track. Trim it to end at exactly 0:03. Cut the original ad at 0:03 and delete everything before that cut. Butt the rest of the original right after hook B. Keep the original audio track from 0:00 untouched (mute hook B). Export at the original size and frame rate as <ad-name>_hook_B.

## Step 6: Deliver

One line per version: letter, hook type, the file or link. Then one line on testing: run A to E as separate ads in one ad set with the same budget, and judge them on 3-second view rate. Say these are starting rules, not guarantees.

## Check before showing

Reject and re-roll once if any of these is true:
1. The hook does not show the beat in its concept.
2. The product looks different from the product in the ad (shape, label, color).
3. The look jumps at 0:03: different light, grade, setting or camera style than the shot it cuts into.
4. Garbled text, a watermark, or a logo that is not the brand's.
5. Broken hands, faces or physics at normal viewing size.
6. Spliced file: size, fps or length differs from the original, or the audio drops or doubles at 0:03.

Still failing after one re-roll: deliver the rest and say plainly which letter missed and why.
