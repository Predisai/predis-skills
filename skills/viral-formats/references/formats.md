# Viral format recipes

Each recipe: why it works, the first-second hook, the shots, the starting pick and cheaper option, a prompt with `{slots}`, typical credits, and posting tips. Credits are from the catalog on 2026-10-08 at 9:16; confirm with `estimate_cost`.

---

## 1. Time-travel POV vlog

**Why:** a modern vlogger voice inside a world nobody alive has seen. The contrast is the joke and the lesson. One era per post makes a series.

**Hook:** the vlogger already mid-sentence, arm out, selfie-stick framing, with something impossible behind them (a half-built pyramid, a longship landing).

**Shots (1 to 3, 8 s each):** arrival with the landmark behind; the camera turns to a local doing something of the era, who reacts; something goes wrong and the vlogger runs.

**Pick:** `veo_3_1`. Cheaper: `veo_3_1_fast` (~51 per shot).

```
Selfie-stick POV vlog, handheld phone camera, slight wide-angle distortion, 9:16.
A {age} {look} travel vlogger in modern clothes ({outfit}) holds the stick at arm's length,
walking through {era_place} in {year}. Behind them: {signature_sight}.
Locals in period dress {local_action}; some stare at the phone.
The vlogger talks to camera, excited and a little out of breath. The vlogger says: {line}
Sound: {ambient}, no music. Natural daylight, realistic skin, documentary feel. (no subtitles)
```

**Credits:** ~169 per shot, ~507 for three. Fast: ~153 for three.

**Posting:** caption as a question ("Which era next?"). Number the series ("Day 3 in Ancient Egypt"). Tight budget: post shot 1 alone as a teaser.

---

## 2. Talking baby or animal podcast

**Why:** a baby or animal giving a confident adult opinion into a podcast mic. The mismatch plus a real opinion starts arguments in the comments.

**Hook:** close-up of the host leaning into a studio mic, already mid-opinion, headphones too big.

**Shots:** one 8 s shot.

**Pick:** `veo_3_1`. Cheaper: `veo_3_1_fast`.

```
Podcast studio, warm practical lights, foam panels, a large studio microphone on a boom arm.
A {host} in oversized headphones sits at the desk and leans into the mic. {host_detail}.
It speaks with total confidence, {delivery}. The {host} says: {hot_take}
Medium close-up, shallow depth of field, slow push in. Sound: room tone, faint mic handling.
Photoreal, 9:16. (no subtitles)
```

Slots: `{host}` "chubby one-year-old baby" or "golden retriever". `{hot_take}` the user's niche opinion, 6 to 12 words.

**Credits:** ~169. Fast ~51.

**Posting:** caption the take as a statement, then "agree?". Pin a comment arguing the other side. Weekly series on one topic.

---

## 3. Street interview with historical figures

**Why:** the familiar sidewalk-mic format, aimed at history. One question, many quick quotable answers.

**Hook:** a fluffy handheld mic pushed toward a recognisable historical figure on a modern street.

**Shots (3 to 5, 8 s each):** same question, same street, same mic, one figure per shot.

**Pick:** `veo_3_1`. Cheaper: `veo_3_1_fast`.

```
Street interview, handheld camera, busy modern city sidewalk at golden hour, passers-by blurred.
A fluffy unbranded microphone is held toward {figure}, dressed in {period_clothes}, {figure_look}.
The interviewer off camera asks: {question}
{figure} answers in character, {delivery}, and says: {answer}
Medium close-up, natural light, city ambience, no music, 9:16, photoreal. (no subtitles)
```

Only long-dead figures. No figures known for atrocities. Caption says "AI-generated". `{answer}` 6 to 12 words, ideally a twist on what they are known for.

**Credits:** ~169 per figure, ~676 for four. Fast ~204. Needs a join (SKILL.md step 5), or post one figure per part.

**Posting:** the question as the first on-screen text (added in the app). Caption: "Who had the best answer?"

---

## 4. Bullet-time freeze

**Why:** chaos stops dead and the camera glides around it. It looks impossible, so people rewatch. Loops well.

**Hook:** half a second of real motion, then everything freezes mid-air.

**Shots:** one shot of about 5 s: motion, freeze, slow half orbit, optional unfreeze on the last beat.

**Pick:** `seedance_2_0`. Cheaper: `seedance_2_0_fast` (~49).

```
{scene}. Real motion for the first beat, then time freezes completely:
{frozen_details} hang motionless in the air.
The camera makes a smooth half orbit around {subject} at chest height,
revealing every frozen droplet and particle. {subject} stays perfectly still, {expression}.
Crisp detail, {palette}, 9:16, no text. Sound: a sudden hush at the freeze.
```

**Credits:** ~61. Fast ~49.

**Posting:** no text on the video, let the freeze surprise. Add a trending sound with a drop on the freeze. Caption: "wait for it".

---

## 5. Glass-fruit ASMR

**Why:** a knife slicing fruit made of glass, with crisp crackle and ring. Satisfying, loops, needs no language.

**Hook:** the blade already touching the glass; the first crack lands in the first frame.

**Shots:** one 8 s macro shot.

**Pick:** `veo_3_1` (the sound is the point). Cheaper: `veo_3_1_fast`.

```
Macro ASMR shot on a dark wooden cutting board, soft top light, 45-degree angle.
A {object} made entirely of clear glossy {glass_color} glass, with visible inner {inner_detail}.
A sharp chef's knife slices it slowly into thin pieces; each slice cracks and rings like crystal,
tiny glass crumbs scatter. Hands in black gloves, slow and steady.
Sound: crisp glass crackle, knife scrape, pieces clinking on wood. No music, no voice.
Photoreal, shallow depth of field, 9:16.
```

**Credits:** ~169. Fast ~51.

**Posting:** caption "sound on". One object a day; ask the comments what to cut next. Keep it under 10 s so it loops.

---

## 6. Tiny-worlds miniature

**Why:** a real place or job rebuilt as a tilt-shift diorama with tiny workers. Cosy, detailed, pause-and-zoom bait.

**Hook:** a giant everyday object (a coffee cup, a pencil) at the edge of the miniature, showing scale at once.

**Shots:** one 5 to 10 s shot, slow move over the diorama.

**Pick:** `kling_3_standard` (~68 for 5 s with audio off, ~102 with audio). Cheap route: `nano_banana_pro` still (~7), then `wan_2_2_i2v_fast` with the still as first frame (~9).

```
Tilt-shift miniature diorama of {place_or_job}, built on {surface}.
Tiny figurine people {tiny_action}, little vehicles move, soft handmade textures,
felt and painted resin. A real {giant_object} sits at the edge for scale.
Slow overhead dolly across the scene from left to right, strong tilt-shift blur top and bottom,
warm morning light, {palette}, 9:16, no text.
```

On the cheap route, the still prompt is the first four lines; the motion prompt is only "tiny figures keep working, slow overhead dolly from left to right, keep the scene unchanged".

**Posting:** caption "which job should I shrink next?". Pairs with lo-fi or music-box sounds added in the app.

---

## 7. "What if" trailer

**Why:** a big alternate-history or sci-fi premise sold with trailer grammar. Feels expensive and invites debate.

**Hook:** the most striking wide shot of the changed world, with "WHAT IF {premise}" as text added in the app.

**Shots:** one 15 s clip with four labeled shots: establishing wide, a human-scale moment, escalation, a final striking image.

**Pick:** `seedance_2_0`, duration 15 (multi-shot in one clip, no join needed). Cheaper: `seedance_2_0_fast`.

```
Cinematic trailer for: what if {premise}. Same look in every shot: {style_line}. 9:16, no text.
Shot 1: {wide_shot}. Camera: slow push in. Sound: low drone.
Shot 2: {human_moment}. Camera: handheld close-up. Sound: {detail_sound}.
Shot 3: {escalation}. Camera: fast tracking. Sound: rising percussion.
Shot 4: {final_image}. Camera: locked off. Sound: one deep hit, then silence.
```

`{style_line}` example: "anamorphic lens, fine film grain, teal and amber grade, hard contrast".

**Credits:** ~184 for 15 s. Over 150, so always ask.

**Posting:** title text over the first 2 s in the app. Caption as the premise question. "Part 2?" in the pinned comment.

---

## 8. Absurd-character brainrot

**Why:** one bizarre hybrid creature with a loud made-up name doing something dramatic. The character becomes the series.

**Hook:** the creature head-on, filling the frame, its name in big text.

**Shots (1 to 3, 5 s each):** reveal, the absurd action, a close-up stare.

**Pick:** `nano_banana_pro` for the still, then `wan_2_2_i2v_fast` with the still as first frame. Cheaper still: `nano_banana_2`.

Still:
```
A surreal hybrid creature: a {animal} fused with a {object}, {material_detail}.
Glossy 3D render, very detailed, bold saturated colors, strong rim light,
centered full body, plain {bg_color} background, 9:16.
Big bold text at the top reading exactly "{NAME}". No other text.
```

Motion (one per shot, still as first frame):
```
The creature {absurd_action}, exaggerated cartoon physics, camera {move}.
Keep its design exactly as the first frame. Single continuous shot.
```

**Credits:** ~7 for the still plus ~9 per shot. Three shots ~34.

**Posting:** the name in the caption and the first frame. Same character every post. Wan output is silent: add a trending sound in the app.

---

## 9. Pet doing a human job

**Why:** a pet played straight in a serious job (barista, news anchor, surgeon). Wholesome and absurd; owners tag each other.

**Hook:** the pet already at work, fully in role, looking at camera as if the viewer is the customer.

**Shots:** one 8 s shot from the customer's view.

**Pick:** `veo_3_1`. For the user's own pet, pass their photo as a reference image (Veo accepts up to 3 and locks duration to 8 s in that mode). Cheaper: `veo_3_1_fast`, or silent `kling_3_standard` with text added in the app.

```
Customer point of view, eye level, {workplace}, realistic light and props.
A {pet_description} wearing {uniform} stands behind {station}, fully focused: {job_action}.
It looks up at the camera, {delivery}, and says: {line}
Sound: {ambient}. Photoreal, shallow depth of field, 9:16. (no subtitles)
```

With a photo, start with "The pet is the exact animal from image 1: same breed, color and markings."

**Credits:** ~169. Fast ~51.

**Posting:** caption in first person as the pet. Ask for the next job in the comments.

---

## 10. Dance-swap onto a trend clip

**Why:** the user, their pet or a character does a trending dance from one photo. It rides a sound the algorithm already pushes.

**Hook:** the first move of the trend, already in motion, full body in frame.

**Pick:** `dreamactor_m2`. It needs exactly one image and one driving video, takes no other options, and picks its own length. No cheaper option.

**Inputs:**
- Photo: full body, facing camera, arms visible, plain background, one person who agreed.
- Dance clip: one dancer, full body, steady camera, a clip the user has rights to. Frame the photo the same way as the clip (both full length).

**Prompt (if the model takes one):**
```
The person in the image performs the exact dance from the video.
Keep their face, outfit and body shape from the image. Full body, even light.
```

**Credits:** about 4 per second of the driving clip.

**Posting:** the output may have no usable sound; add the original trend sound in the app. Post while the trend is still rising. Use the trend's hashtag.
