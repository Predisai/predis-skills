# Prompting for carousel

Rules for the two models this skill uses. Option names and allowed values always come from `describe_model`; the values below were live when this was written and may change.

## General

- Write the slide like a designer's brief: background, grid, where each text block sits, size, weight, color, margins.
- Exact text goes in double quotes. Readable typefaces only; decorative lettering fails first.
- Colors as hex. Name the font feel (bold serif, geometric sans, condensed sans), not a font file.
- Keep generous margins: nothing within about 8% of any edge, so platform crops and UI do not cut it.
- Change one thing per retry.

## gpt_image_2 (starting pick)

Options seen: `output_size` (includes `1024x1024`; also 1024x1536, 768x1024, 1080x1920 and others; no 4:5), `quality` (low, medium, high). Up to 5 image references.

Strong at exact on-image text and clean graphic layouts.

Slide 1, which sets the look:

```
Square social carousel slide, flat graphic design, no photo.
Background: solid {hex}. Generous margins on all sides.
Headline "{hook}" in a large bold {font feel}, {hex}, left aligned, upper half.
Small text "swipe" with a thin arrow, bottom right, {hex}.
{Optional: one simple flat illustration of {object}, lower right, in {hex} and {hex}.}
No other text, no logos, no watermark.
```

Slides 2 to 7, matched to slide 1:

```
Image 1 is slide 1 of this carousel. Match its background, colors, typeface, text sizes, margins and layout system exactly.
Change only the text and the small visual.
Title "{title}" in the same headline style, upper half, left aligned.
Body "{line}" in a smaller regular weight of the same typeface, under the title.
{Optional: small flat illustration of {object}, same style as image 1.}
No other text, no logos, no watermark.
```

CTA slide: same lock line, then the CTA text, then `the logo from image 2, small, bottom center` if there is one.

Focused text fix: pass the slide as image 1. `Change only the {title or body} text to exactly "{copy}". Keep everything else identical.`

## nano_banana_pro (for 4:5)

Options seen: `aspect` (1:1, 4:5, 16:9, 9:16), `resolution` (1K, 2K, 4K). Up to 14 image references.

Use only when the user wants 4:5. Its text is good for a few words but slips sooner than gpt_image_2, so keep body lines short (about 12 words). Same templates, with `4:5` in place of "square". Spell any hard word once: `the word "MONSTERA" spelled M-O-N-S-T-E-R-A`.

## What not to write

- "8k", "award winning", "stunning": they add nothing.
- Facts or numbers that are not in the source.
- Real people, other brands' logos or characters.
