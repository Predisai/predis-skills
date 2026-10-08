---
name: headline-variants
description: >-
  Takes the user's static image ad and makes 6 versions where only the headline changes. Writes 6 headlines
  from 6 different angles, gets one OK, then edits the headline text on the original ad with a precise image
  edit, so the visual, product, layout and every other word stay locked. Outputs are named A to F so the
  headline is the only variable in a split test. Use when: "/headline-variants", "make 6 headline variants of
  this ad", "test headlines on this image", "same visual, different headline", "new copy on my static ad",
  "A/B test the headline". NOT for: new ads from a product link (link-to-ads), UGC creator videos (ugc-ad), a
  product video (product-video), remaking an ad you like (reference-ad), a swipe carousel (carousel), new
  openings for a video ad (hook-variants), changing a shot in a video (fix-frame), a video thumbnail
  (thumbnail), trending social formats (viral-formats).
---

# Headline variants

Same image, 6 headlines. Everything except the headline text is locked, so when one version wins, the user knows the headline did it.

First, read [references/predis-mcp.md](references/predis-mcp.md) and follow its loop for every generation. Model prompting rules are in [references/prompting.md](references/prompting.md).

## Step 1: Get the ad

Ask for the static ad if it is not in the chat. In the same message, ask only what you cannot see:
- What the product is and who it is for, if the ad does not say.
- Any facts you may use: reviews, real numbers, a real offer (optional).

Look at the ad and write down:
- The current headline, word for word.
- Where it sits, its font style, weight, color, case, and roughly how many characters fit on each line.
- Every other piece of text on the image (subhead, CTA, logo, badges). These must not change.

If the ad has no clear headline, ask which text to swap. Don't guess.

## Step 2: Write 6 headlines

Each one takes a different angle. Pick 6 that fit the product:

| Angle | Shape |
|---|---|
| Pain | "Stop [annoying thing]" |
| Outcome | "[Result] in [time]" |
| Proof | "[Real number] people switched" (only with a real number) |
| Curiosity | "The [thing] nobody tells you about [topic]" |
| Compare | "Unlike [category], this [does X]" |
| Identity | "Made for [specific person]" |
| Contrarian | "Why [common advice] is wrong" |
| Offer | "[Real offer], this week only" (only with a real offer) |

Rules:
- Every claim comes from the ad, the product page the user shared, or the user. No invented stats, reviews or offers.
- Keep each headline within about 20% of the current headline's length. Much longer text forces a new layout, and the layout must not move.
- Plain words. One idea per headline.

Show a short table: letter, angle, headline. Add the total cost (Step 3) and ask: "Good to go, or want to change any?"

## Step 3: Price and get one OK

Starting pick: `ideogram_4_5_precise_edit`. It takes the ad as the source image and edits it in place. Set `quality` to `high` for finals. Check its options with `describe_model` first.

Fallback if it is missing or keeps changing other parts: `gpt_image_2` with the ad as image 1 (pick the `output_size` with the ad's ratio) or `nano_banana_pro` (`aspect` matching the ad). Both redraw the whole image, so check them harder.

Run `estimate_cost` once and multiply by 6. One OK covers the whole job.

## Step 4: Edit

- Reference: the original ad as the source (image 1). Never a previous variant: always edit the original, so errors don't stack.
- One edit per headline, written with [references/prompting.md](references/prompting.md).
- Six takes in one batch, each with its own idempotency key.

## Step 5: Check every output against the original

Open each result next to the original ad and compare, region by region: product, people, background, colors, every other line of text, logo, CTA, crop and size. Then read the new headline letter by letter.

If you can run ffmpeg, a difference image shows anything that moved. Bright areas outside the headline mean a hard fail.

```bash
ffmpeg -y -i original.png -i headline_B.png -filter_complex "[1:v][0:v]scale2ref[b][a];[a][b]blend=all_mode=difference,eq=contrast=4" -frames:v 1 diff_B.png
```

## Check before showing

Hard fail. Re-run that letter once with a stricter prompt, then with the fallback model:
1. Anything other than the headline text changed: product, people, background, colors, logo, CTA, other copy, crop or size.
2. The headline is misspelled, garbled, or differs by even one character from the approved line.
3. The headline font, color, size or position does not match the original.
4. The headline collides with other elements, gets cut off, or is hard to read at phone size.
5. A claim on the image is not backed by the ad or the user.

Still failing after two tries: leave that letter out, say which one and why, and offer a shorter headline for it.

## Step 6: Deliver

Name files `<ad-name>_headline_A.png` to `_F.png`, in the same format and size as the original. With a shell, save them in `./predis-output/headline-variants/<ad-name>/`; without one, share the links in A to F order.

One line per version: letter, angle, headline. Then one line on testing: run A to F as separate ads in one ad set with the same budget and the same primary text, and judge them on click-through rate. Say these are starting rules, not guarantees.
