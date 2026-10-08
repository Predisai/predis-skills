# Prompting the hook clips

Starting pick: `seedance_2_0_fast`. Upgrade: `seedance_2_0`. Options and reference wording come from `describe_model`; if it describes references differently from below, use its wording and keep the same roles.

## Shape of a hook prompt

About 40 to 90 words. Subject and action first.

`[Subject] [one action with an endpoint], [where]. Camera: [one move, start and end]. Light and look: match image 1, same setting, grade and camera style. [Product line]. Single continuous shot, no cuts. No on-screen text, no subtitles, no watermark.`

- Product line, when the product is in shot: "The product from image 1, same shape, label and color."
- Image 1 sets the look and the world. Say so: "Image 1 is a frame from the ad; match its setting, light and grade."
- If you passed the ad as video 1 instead: "Video 1 sets the look only; do not copy its opening."

## Rules that matter here

- One beat per hook. Three seconds holds one action, not a story.
- Put the beat in the first second. The thumb-stop must land before 0:01.
- End on a settled frame. The clip is trimmed at 3s and cut straight into the body, so nothing should be mid-whip at the cut.
- Match the camera of the ad. Phone ad: "handheld smartphone footage, natural light". Studio ad: name the move and the light source.
- Describe what is there, not what is not. "Hands rest on the counter" beats "no extra fingers".
- Skip empty words: cinematic, epic, stunning, 8K. Name the shot size, move and light instead.
- Do not re-describe what image 1 already shows. That fights the reference and causes drift.

## Examples (one per type)

- Question: "A woman in a grey hoodie stares at a tangle of charging cables on her desk, then looks up at the camera. Camera: static, eye level, medium close-up. Match image 1's room light and grade..."
- Proof first: "Close-up of the finished result held up to the window, then the hand turns it toward the lens. Camera: slow push-in, ends on the result filling the frame..."
- Pattern break: "The product from image 1 slides into frame upside down on a tilted table and stops at center. Camera: static, top-down..."

## If a take fails

- Product drifts: shorten the prompt, keep only the product line and the action.
- Looks nothing like the ad: move "match image 1" to the start of the prompt.
- Too still: add one physical action and when it happens ("in the first second").
