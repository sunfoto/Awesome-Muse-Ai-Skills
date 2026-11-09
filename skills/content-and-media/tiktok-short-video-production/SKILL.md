---
name: tiktok-short-video-production
description: Produce short TikTok films with a sample approved first, readable subtitles inside the platform safe area, and a verified posting check.
---

# Produce short videos for TikTok

## Approve a short sample before the full film

- Build a sample of about 20 seconds in the chosen style and get the user approval before you build the full film.

- Judge voice and picture separately and keep the part the user approved when the other part is rejected.

- Offer a new sample when a style is rejected and never rebuild a full film on a guessed style.

- Present style choices from the house code-film engine (a catalogue of numbered code-drawn film styles) and name the style number with the sample so the approval refers to one exact style.

## Choose the voice and keep it separate from the picture

- Get the voice approved on its own sample before you lock it to a full film.

- When you use VieNeu-TTS on this machine, download the model with snapshot_download to a real directory and pass that directory through Vieneu with onnx_dir instead of relying on the Hugging Face cache.

- When you use VieNeu-TTS, set the codec directory through the codec_dir monkeypatch for the MOSS codec and allow about three times realtime for rendering.

- Record the chosen voice by name in the project notes and use that same voice for the full film unless the user orders a change.

## Keep subtitles readable and inside the safe area

- Limit each subtitle panel to two lines and shorten the wording when a third line would wrap.

- Keep captions in the safe area above the platform overlay and away from the right hand buttons.

- Raise the stage to about three quarters of the frame height and narrow it on the left when text is hidden behind the platform overlay.

- Verify the exact file you will deliver and never assume a same name file is the new version.

- Recheck the affected second with a frame grab after a subtitle fix and send the corrected file again with that proof.

## Check the master and post it with verification

- Check the master loudness at minus 14 LUFS within one LUFS and true peak at or below minus 1 dB before delivery.

- Post through TikTok Studio and expect a QR login when the session needs one.

- Post private first and wait about 10 minutes for review before you switch the video to public.

- Turn Duet and Stitch off when the video is longer than 60 seconds.

- Treat a CAPTCHA or a profile check as the user action and never claim a video is posted until you have verified it at the top of the posted list in TikTok Studio with its caption intact.

## Hybrid build: code-drawn scenes plus AI people (owner-approved 2026-11-06 on a house ink-wash short)

Use this structure when code-drawn figures are not lifelike enough: keep the house code-film engine for scenes, replace the people with AI-generated ink images, and animate real motion with generated frame cycles.

- Detail pass on the code film first: denser brush fibres and finer strokes, a second wash layer on edges, more paper grain, two-layer rain (far and near) with ground splashes, and detailed props (signage, cookware, furniture, wires, lit window, puddles reflecting the accent light). Keep one single colour accent for the whole film (red lantern plus seal in the ink-wash films).

- Render at 60 fps and remove any low-fps quantisation of secondary motion (the ink films used a 12 fps step) so rain, smoke and swaying move smoothly.

- Lock the cast before generating scene art: get one full-scene sample image with the main characters approved by the user first, then reuse that image as the reference plus identical written character descriptions (hair, jacket, cross-body bag, shawl, cane) in every later generation. Introduce a new character with its own approved reference image before using it in scenes.

- Generate people as isolated full figures on plain white paper, ink only with no colour accent. Standing figures take one image plus a subtle bob; moving figures take a frame cycle: an 8-frame walk cycle (contact, weight, passing, reach, then mirrored), a 4-frame slow shuffle for an elderly figure, short arm cycles for actions such as ladling.

- Generate cycle frames chained: each frame takes the previous frame as its reference so the face and outfit stay one person. Regenerate a frame that visibly drifts (wrong leg pose, running pose in a walk cycle) instead of accepting it.

- Key the paper away by ink density: alpha from darkness, RGB forced to the film ink colour, a small knee to kill source paper grain, light feather. Register cycle frames with PIL onto one shared canvas: same normalised height, same ground baseline, same upper-body anchor, so switching frames does not jump.

- Composite in film.js where the code figure stood, at the layout's position and scale. Draw the code rain over the AI people so they sit inside the weather, add a soft contact shadow under the feet, and drive the walk cycle phase from travelled distance (one cycle equals two measured strides) with the vertical bob synced to the weight and reach frames, so feet do not slide.

- Approval gates in order: cast sample image, then a roughly 20 second hybrid sample, then the full build. When a sample is rejected, ask for the exact failing point before rebuilding (the v1 hybrid was rejected only for static people, which this cycle step fixes).

- Keep the approved voice and mix unchanged, mux to minus 14 LUFS, and treat posting as a separate user decision even when a lower-quality version is already live.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with the Custom Connectors and MCPs skill, or build a custom connector.

## Alternatives for better results

- Show one short sample per style instead of describing several styles in words when the user has not picked a style.

- Split voice approval and picture approval into two short checks when either one is uncertain.

- Reuse an approved sample as the style reference for the next film in the same series instead of starting the style choice again.