# Prompting the headline edit

Starting pick: `ideogram_4_5_precise_edit` (source image plus an edit instruction). Fallbacks: `gpt_image_2`, `nano_banana_pro`. Option names and reference slots come from `describe_model`.

## The edit prompt

Short and literal. Long edit prompts invite the model to change more than you asked.

```
Change the headline text "OLD HEADLINE" to "NEW HEADLINE".
Keep the same font, weight, color, size, case, alignment and position.
Change nothing else: same image, product, background, colors, logo, button and all other text, same crop.
```

- Quote both the old and the new text exactly, in double quotes.
- Match the case of the original. If the ad uses all caps, write the new line in all caps.
- If the old headline breaks over two lines, say where the new one breaks: `on two lines: "STOP WAKING UP" / "AT 3AM"`.
- Keep the new line close in length to the old one. A much longer line gets squeezed or spills into other elements.

## On the fallback models

`gpt_image_2` and `nano_banana_pro` redraw the full image from the reference, so the lock line matters more. Lead with it:

```
Image 1 is a finished ad. Reproduce it exactly, pixel for pixel in layout, product, colors and every element,
with one change: the headline "OLD HEADLINE" now reads "NEW HEADLINE", in the same font, color, size and position.
```

Set the size to match the original: `output_size` on `gpt_image_2`, `aspect` on `nano_banana_pro`.

## If a take fails

- Other text changed too: name the text that must stay, in quotes ("keep the button text \"Shop now\" exactly").
- Font changed: describe it ("bold condensed sans-serif, white, all caps").
- Letters garbled: shorten the headline or use plainer words; decorative or very long lines fail first.
- Layout moved: go back to the original as the source and retry; never edit an edited variant.
