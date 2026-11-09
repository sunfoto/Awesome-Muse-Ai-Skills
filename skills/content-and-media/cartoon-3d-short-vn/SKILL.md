---
name: cartoon-3d-short-vn
description: Make a 55–70 second vertical cartoon 3D story short (Vietnamese adaptation of the MIT-licensed cartoon-3d-short skill): flat 2D bean-head cartoon characters inside a colourful three.js world rendered with Remotion at 30 fps, an 8-beat fact-first script paced for Vietnamese narration, one locked narrator voice, and synthesized SFX and music.
---

# Make 60-second cartoon 3D story shorts (Vietnamese adaptation)

## Provenance and licence

- This skill adapts **cartoon-3d-short** from the open-source repo `mromidev/claude-animated-shorts` (MIT License, © the repo authors) — clone that repo locally before starting; the story scaffolding lives in the clone. The original `LICENSE` file is copied into this skill folder and covers the vendored templates.
- Original proof: "The Bridge That Fell for a Song" (Broughton bridge, 1831). House proof: a Tacoma Narrows (1940) trial build — every adaptation below was verified on a real trial build on an 8 GB machine. Keep a production-notes file for your own trial build and read it before the first real episode.
- The upstream reference frames and reports are private study material in the original skill: use them for the look, never copy their scripts, characters or shots, and never upload or publish them. Story, characters and voice are always our own.
- Series home: keep one folder per episode (script in `KICH-BAN.md`, topic file in `CHU-DE.md`).

## What this format is

- A 55–70 second vertical (1080×1920, **30 fps**) animated short telling ONE surprising true story with a visible physical cause: flat 2D bean-head cartoon characters (Canvas2D billboards, ink outlines) inside a colourful 3D world (three.js inside Remotion), one steady narrator voice, synthesized SFX and music, **no captions or text inside the video**.
- It suits true-event/science shorts (explainer-style 60 s stories). It does NOT replace a long-form manhua pipeline (long-form storytelling, fixed cast); borrow its two timing laws for other pipelines instead: visual reactions land 0–3 frames BEFORE their word, and no frame may sit dead for 4+ frames.

## Locked numbers

- Duration 55–70 s; voice starts at frame 1–4; no intro, logo, outro or CTA; the video ends on the last word.
- Narration pace in Vietnamese: **~4.25 words/second** (measured on the Tacoma trial with the house locked voice). Do NOT force the original 3.7 words/s English pace onto Vietnamese — Vietnamese words are mostly monosyllabic and 3.7 sounds dragging. Budget the script at roughly 235–290 words for 55–70 s and verify against the real recorded voice length.
- Script structure, 8 beats over ~11 lines: **Hook → Setting → Escalation → Snap → Luckily → Unluckily → Explanation → (everyday Analogy) → Consequence → Callback**. One line = one voice file.
- Camera: hard cuts only on big place/time jumps; everything else is a smootherstep glide of **18–40 frames**; every hold creeps in at 0.15–0.35 %/frame (0.6 %/frame on punch words); impact kicks flip sign each frame and decay over 7 frames; rolls of 4–5°.
- Characters: about **3.5 head-heights** tall, bean head (h/w ~0.94), plum ink `#241726`, stick limbs, no blush, no eye highlights; walk bob 3–7 %, blinks every 0.5–2.3 s.
- Final loudness (house channel standard, REPLACES the original mix numbers): **−14 LUFS ±1 integrated, true peak ≤ −1 dBFS**, measured on the final stable file. Budget ~0.5 dB of headroom before the AAC encode, which lifts the true peak (the trial's first loudnorm pass measured −0.97 TP after encode and had to be trimmed and re-measured).

## Pipeline (11 steps, Vietnamese adaptation)

1. **Topic and facts.** Pick one surprising TRUE story with a physical cause and effect you can SEE, plus one everyday analogy that explains it. Verify every fact against at least two trustworthy sources and write the facts with their sources at the top of `KICH-BAN.md` (and in `CHU-DE.md`). If the official cause was never formally determined, say so in the notes and phrase the script as the widely reported explanation, never as settled fact. Never invent numbers.
2. **Script** in `KICH-BAN.md`: hook = the absurd fact stated plainly in line 1, no setup; setting in 1–2 lines (year, place, concrete numbers); escalation in 2–3 beats, each bigger; the Snap; the deadpan pair "May là… / Xui là…"; the explanation through ONE physics idea; the analogy ("Giống hệt lúc…"); the consequence/rule it created; the callback twist on the last line. Spell numbers out as words in the narration lines (Vietnamese TTS reads digits inconsistently). Never describe on screen what the viewer already sees. Count the words with a script and record the real count. Get the user's approval of topic + script before producing anything.
3. **Voice.** Generate one WAV per line with **VieNeu-TTS** (open-source Vietnamese TTS), using the single voice locked for the series in its series bible — use your own voice, and never publish a personal voice ID in a shared or public document. Setup traps on a CPU-only machine: install into a local venv, download the model with `snapshot_download` to a real directory and pass that directory as `onnx_dir` (ONNX Runtime rejects the model inside the Hugging Face cache), and set the MOSS codec directory through the `codec_dir` monkeypatch (see the tiktok-short-video-production skill). Expect ~3× realtime on CPU (~4 minutes for 11 lines).
4. **words.json without real timestamps.** VieNeu does not emit word timings, so split each line's measured duration across its words proportionally to word length. Accuracy is about ±0.2 s — hand-check and hand-fix the timings of the anchor words that trigger visual reactions. Everything in the picture is timed from words.json.
5. **Shot plan** in `story.ts`: copy the bridge template from the upstream clone (`templates/bridge/` in the cloned repo) into the episode's Remotion project as `src/videos/<name>/`, register the composition (1080×1920, 30 fps) in `src/Root.tsx`. Declare SHOTS, XFADE (crossfades at place/time changes), FLASH_AT (white flash after the hook), CAMS keys, KICKS, DOF per beat, FOREGROUND props in ~1/3 of shots. Every visual reaction lands **0–3 frames BEFORE its word**.
6. **World.** Build the smallest world that tells the story (one main set, plus a small analogy set). Reuse the vendored `templates/toon.ts` character API and `shade.ts`/`cardshade.ts` from THIS skill folder verbatim — the bean-head look lives there. Recolour any CC0 kit props by material name (Kenney nature kit ships teal). Run the sight-line culling idea from the original: remove anything that blocks a camera ray when you move cameras.
7. **Post — REDUCED on a small machine.** The original composer chain (ink shader → lens blur → bloom → NEUTRAL tone map → grade) is too heavy for an 8 GB VM. House version: ink via `EdgesGeometry`/`LineSegments`, no lens blur, no bloom, character cards as `MeshBasicMaterial` with a warm tint, interior sets (tunnels, rooms) also `MeshBasicMaterial` (Lambert interiors render nearly black here). This keeps ~90 % of the feel at a fraction of the cost. Brighten material colours one step above your instinct: sRGB→linear conversion plus tone mapping darkens mid-greys visibly (the trial's bridge deck had to go to `#b7bcbf`/`#ccd4d9` to read correctly).
8. **Sound.** Synthesize SFX with numpy (vendored `templates/dsp.py` + `templates/instruments.py` carry the original formulas: wind, creaks, cracks, splashes, geysers) and compose a simple original score; duck the music under the voice envelope. Music and SFX must be self-synthesized or clearly licensed — never lift tracks. Lock the picture's timing constants BEFORE making sound; any later timing change means re-running the sound step. Mux with `-map 0:v:0 -map 1:a:0` (Remotion writes a silent audio track) and loudnorm to the house standard above.
9. **Look checks.** Render stills at the anchor moments (hook, setting, escalation, snap, explanation, callback) and view each one yourself before the full render. Check the readability traps at the end of this file on every new prop and shot.
10. **Final render — the bottleneck.** Render with Remotion at concurrency 1 on a small VM. Point `--browser-executable` at the Playwright headless shell already on the machine: downloading Remotion's own Chrome can stall on a slow network. Expect **~90 minutes for a ~59 s video** (~3 s/frame, measured on the Tacoma trial on an 8 GB VM with another job sharing the machine half the time). Set `TEMP`/`TMP` to a roomy folder: every Remotion bundle copies `public/` (~1.3 GB) into the temp dir and never deletes it; clean `remotion-webpack-bundle-*` folders after the project is done and nothing is rendering.
11. **QA and deliver.** ffprobe the final file (1080×1920, 30 fps, expected duration), run blackdetect (zero black segments), measure LUFS/TP on the final stable file (wait for every writer process to exit, measure twice), extract QA frames from the FINAL file and view them. Deliver the mp4 plus the production notes; posting is a separate user decision.

## Cutting the render cost (known levers, in order)

- Render overnight or in a batch queue; treat ~1.5–2 h per episode as the planning number on this VM until a stronger render machine exists.
- Keep the reduced post chain (step 7); lens blur and bloom are the most expensive passes and the first to re-enable only on a stronger machine.
- Build ONE reusable town/world set and one townsfolk cast per series so each episode only rebuilds its hero set.
- Reuse stills QA aggressively: a 60–100 s still (including ~40 s of bundling) is far cheaper than discovering a bad shot after a 90-minute render.
- Keep scenes geometrically simple: boxes + line cables read fine in this style; detail belongs in colour, ink and motion, not polygon count.

## Fact and rights rules

- True stories only, each fact traceable to a named source in the episode files; two independent trustworthy sources minimum for the main topic.
- Music and SFX: synthesized in-house or openly licensed with the credit recorded; never copy the reference channel's scripts, characters, shots or name (do not name it in voice-over or descriptions).
- Character designs come from the vendored `toon.ts` (MIT) plus our own outfits and stories.

## Templates: what is vendored here and what stays in the clone

- Vendored in this skill's `templates/` (copied verbatim, MIT, see `LICENSE`): `toon.ts` (the character drawing API — Pen, bodyFront/bodySide, swingKid, splash/ring/burst effects; the heart of the look, reused unchanged every episode), `shade.ts` and `cardshade.ts` (two-tone card shading), `dsp.py` and `instruments.py` (numpy sound synthesis core).
- Referenced in the upstream clone: `templates/bridge/` story scaffolding (story.ts, build.ts, world.ts, lights.ts, paint.ts, ink.ts, lensblur.ts, grade.ts, Bridge.tsx) — copy per episode and rewrite for the story, so vendoring it here would only create a stale fork; `templates/audio/` remaining scripts (timeline.py, sfx.py, sfx_gen.py, music.py, mix.py) as formula reference; `templates/scripts/` QA helpers (bridge-stills.mjs, short_qa.py, sbs_sheet.py); `references/STYLE_BIBLE.md` and `references/reports/` for measured look targets.

## Readability traps (from the original verifier and the house trial)

- A swing drawn with one rope reads as a hanging: always two ropes to a wide plank, kid between them, A-frame legs clear of the swing arc.
- Any prop closer than ~2 m to a lens becomes a huge cut-off blob; re-check every prop near a camera after any camera move.
- A foreground bush cut by the frame edge reads as a green ball: a foreground prop must be recognisable or gone.
- Flags: red + yellow star/diamond reads as Vietnam's flag — use period- and place-correct flags only.
- Signs lit by a side sun go grey-blue and unreadable: give sign faces an unlit material (and prefer no text at all in this format).
- Golden rim light as an offset outer copy turns hats into picture frames: inner rim only, head/torso only.
- Rails at chest height cut through characters: raise the lens (~2.5 m) so rails cross legs, not torsos.
- Flat holds read as freezes in QA: every hold creeps, no exceptions.
- Per-frame physics state must reset inside replay/explanation windows — the trial's car kept sinking through the whole explanation replay because its sink value was never reset for that window. Check every time-based state against every window it appears in.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.
- Sister skills: `tiktok-short-video-production` (voice standard, loudness standard, posting verification), `project-image-generation` (if an episode ever needs AI stills — not this format's default).
