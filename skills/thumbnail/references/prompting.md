# Prompting the thumbnail models

Models: `gpt_image_2`, `gpt_image_2_5_flare`, `nano_banana_pro`, `nano_banana_2` (starting picks). Option names and allowed values come from `describe_model`, never from this file.

## Every image model

- Write full sentences, not tag lists.
- Be specific about place: what sits where, which colors, what light from which side.
- Put the must-haves first. The longer the list, the more likely one item gets dropped.
- Get close, then improve with small edits instead of rewriting from scratch.

## Text in the image

- Put the exact words in double quotes, then font style, weight, color and position: `"DAY 30" in a huge heavy sans-serif, yellow with a thick black outline, top left`.
- Plain heavy fonts render best. Fancy lettering breaks first.
- Say `No other text, no watermark.` so the model does not add extra words.
- To change text in an edit: `Change "SALE" to "NEW", keep font, color and position`. Keep the new text about the same length.
- `gpt_image_2` is the default for text. `gpt_image_2_5_flare` when there is a lot of text and it must be perfect.

## GPT Image (`gpt_image_2`, `gpt_image_2_5_flare`)

- Size is set by `output_size` (for example 1920x1080 or 1080x1920), not by words in the prompt.
- Write it like a designer's brief: what is biggest, where the headline sits, the exact copy in quotes, colors.
- Up to 5 reference images. Name each by number and role.
- For edits, name the area, the change, and what stays.

## Nano Banana (`nano_banana_pro`, `nano_banana_2`)

- Size is set by `aspect` (1:1, 4:5, 16:9, 9:16) and `resolution`.
- Best for face likeness and edits. Takes many reference images; number them and give each a role: "image 1 is the man's face, image 2 is the logo".
- Keep face photos clear: front-facing, even light, no sunglasses.
- `nano_banana_2` is the cheaper draft version with the same prompt style.

## Edits (image to image)

- Say what stays: "keep the face, pose, framing and light identical; only replace the background with a night city".
- Use a precise verb. "Change her jacket to red" beats "transform".
- Removing something: say what fills the gap.
- New background: describe its light so the subject still fits.
- One change per pass. Big changes go in steps.
