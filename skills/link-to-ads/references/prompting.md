# Prompting for link-to-ads

Rules for the two models this skill uses. Option names and allowed values always come from `describe_model`; the values below were live when this was written and may change.

## General

- Write full sentences. Say where things sit in the frame, what the light does, what the surface is.
- Put the must-haves first: the product lock, then the headline, then the scene.
- Number references in the order you pass them and give each one a job: "image 1 is the product", "image 2 is the logo".
- Keep the product photo clean (plain background, label facing camera). A busy reference drifts more.
- Change one thing per retry. Rewriting the whole prompt hides what fixed it.

## nano_banana_pro (starting pick)

Options seen: `aspect` (1:1, 4:5, 16:9, 9:16), `resolution` (1K, 2K, 4K). Up to 14 image references.

Best at keeping a real product exact while placing it in a new scene. Handles short on-image text well. Long copy or more than one text zone is where it starts to slip.

Ad template, one per headline:

```
Keep the product exactly as in image 1: same shape, label text, colors and proportions.
4:5 social ad for {brand}. {product_name} {scene for this headline}. {surface and props}.
{light: direction and quality}. Product large and sharp in the {lower or center} of the frame.
Headline reads exactly "{headline}" in a bold {font feel} typeface, {color}, {top third, left aligned}, on a calm area of the image with strong contrast.
STYLE: {brand color 1} and {brand color 2} in the set and props, {one-word feeling}, clean commercial photography.
No other text. No logos except the product's own label{ and the logo from image 2 small in the bottom right}.
```

- Keep the STYLE line word for word across the set. Vary only the scene line.
- For a hard brand word, spell it once: `the word "ACME" spelled A-C-M-E`.
- Fix a single wrong word with an edit instead of a re-roll: pass the take as image 1 (`add_reference(asset_id=...)` with its asset id from `check_run`) and write `Change only the headline to exactly "{headline}". Keep everything else identical.`

## gpt_image_2 (fallback for text)

Options seen: `output_size` (includes `1024x1024`, `1080x1920`, `1024x1536`; no 4:5), `quality` (low, medium, high). Up to 5 image references.

Use it when a headline keeps coming out wrong. Write the prompt like a designer's brief: layout, hierarchy, exact copy in quotes, colors as hex, margins. Keep the same product lock line first.

## What not to write

- "8k", "award winning", "perfect": they add nothing and push a plastic look.
- Prices, discounts or ratings the page does not show.
- Another brand's name or logo.
