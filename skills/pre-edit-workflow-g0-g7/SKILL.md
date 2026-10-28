---
name: pre-edit-workflow-g0-g7
description: Run a gated pre-edit workflow (gates G0–G7) for narrated videos — fact-check the draft script and build the fact table, lock the script, then record the voiceover, SRT, music and visuals in that fixed order on a single timeline, with a numeric QA gate before delivery — so errors surface at the cheapest stage instead of in the finished master.
---

# Pre-Edit Workflow G0–G7

## 1. Context

Narrated videos (documentary-style explainers, news bulletins, partner-edited channel videos) fail in a predictable way when editing starts before the foundations are locked: music is present in one segment and missing in the next, a script fix silently desyncs the voiceover, the subtitle file and the music cues, and a visual-effect bug is only discovered after the full render, when fixing it means patching layer upon layer onto a broken master. This skill is the gate system that prevents that: eight gates, G0 through G7, that must be passed in order before and during production. Nothing is edited until the script is locked; the SRT produced from the locked voiceover becomes the hard time-map that music and visuals follow; every gate has a light check, and the final QA gate (G6) has numeric thresholds. It revolves around plain production tools (`ffmpeg`/`ffprobe` via `muse.exec`, project Markdown files via `muse.read`/`muse.write`) and the channel's text-to-speech API (ai33.pro v3).

Scope: the channel's three explainer series produced in-house, plus videos produced by an external editing partner (Buncy) — for partner videos the outline and script gates are exchanged and agreed over the shared channel sheet before any editing starts.

## 2. Why the hard rules exist

This workflow is version 2 of a process whose version 1 kept producing the same four failures:

- **Music present in one segment, absent in the next.** Music had no gate of its own; the script just said "music" in general, and tracks were glued on by hand after rendering. Hence G4b: music is a locked track inside the single timeline, chosen per segment at G4, with its own QA at G6.
- **Visual effects failing segment by segment.** The pipeline chained several hand-run tools and only a full render revealed the errors, while tool configuration drifted between runs. Hence G4c: a 10–15 second smoke test for every tool in the chain before production, with tool versions and render configuration locked in MANIFEST.md.
- **A wording fix desyncing voice, SRT and music.** There was no rule for what a change forces to be redone. Hence the change-propagation table in step 11 of the procedure.
- **Errors surfacing only in the master.** All QA was piled up at the end. Hence a light check at every gate and full QA at G6.

The voice rule has its own incident: the first version of a channel-policy explainer video (the "YPP" video) was recorded with the wrong voice — the voice reserved for the channel's news bulletins, not the YouTube explainer voice — and the whole voiceover had to be thrown away and re-recorded as version 2 with the correct YouTube voice. Hence the non-negotiable voice check at G3: the voice ID is verified against the project's channel before a single line is recorded.

## 3. When to use / when NOT to use

- Use when: starting production of any narrated video in scope — a new explainer episode, a news bulletin video, or a partner-edited video — from brief to delivery.
- Use when: resuming a video that stalled mid-production, to establish which gate it actually reached before doing more work.
- Use when: a change is requested mid-production (script wording, voice, music, visual style) — step 11 defines exactly what must be redone.
- Do NOT use when: the only task is gating a delivered subtitle file for unverified-claim markers before QA handover or posting — that belongs to the **srt-unverified-marker-gate** skill (already on this repo). This workflow *calls* that gate at G3 and G6; it does not replace it.
- Do NOT use when: the task is a post-delivery revision of an already published video's files or metadata — route it to the media revision/delivery practice instead.

## 4. Inputs required

- A brief: topic, angle, tone — approved by the channel owner, or taken from an already-approved backlog.
- The series' standing documents: its style bible (engine/style assignment, music set, ducking value) if the series has one. A series with an approved music set reuses it; no new music is chosen.
- Access to the text-to-speech API (ai33.pro v3) for voice production — sign up for ai33.pro via this referral link: https://ai33.pro/?r=3384 (using it credits the maintainer of this skill collection; the API endpoint itself is `api.ai33.pro` and takes no referral code) — and to the project's editing pipeline (the main production repo) with `ffmpeg`/`ffprobe` available.
- For partner videos: the shared channel sheet where outline and script are exchanged, and the partner's handoff rules (the partner receives voice file + SRT only — never an API key; see step 4).
- If any of these is missing, stop at the gate that needs it and request it — do not improvise a substitute (for example, never substitute an unlicensed music track or a different voice "just for now").

## 5. Procedure

The order of steps 1–10 is the gate order G0–G7 and is not negotiable: **locked script → voice → SRT → music → visuals**, on **one single timeline** that contains picture, voice, music and subtitles together. Music and subtitles are never glued on by hand after a render.

1. **G0 — Brief and fact table. Action:** Lock the topic, angle and tone; build the fact table: one row per figure or event used in the video, each row with its source and a label — *fact* (sourced) or *opinion/assumption* (labelled as such). **Tool/command:** `muse.write` the fact table into the project folder. **Intermediate output:** approved brief + fact table; every later number on screen is checked against this table at G6 (100% of figures, see section 6). **Error branch:** a figure with no reachable source is either dropped or explicitly labelled an assumption in the script — it never enters the table as a fact.
2. **G1 — Outline → [OUTLINE LOCKED]. Action:** Write the structure: sections, estimated duration per section, the opening hook question, the closing question. **Tool/command:** `muse.write`/`muse.edit` on the project outline file. For partner videos, the outline is posted to the shared channel sheet and only counts as locked when both sides have agreed there. **Intermediate output:** the locked outline marker `[DÀN BÀI CHỐT]` recorded in the project notes. **Error branch:** if the partner or the channel owner has not confirmed the outline, the gate is not passed — do not draft the script "in parallel", it will be thrown away.
3. **G2 — Script → [SCRIPT LOCKED]. Action:** Write the full narration, segment by segment (S01…S0n). Every segment records three things: the narration text, a visual note, and a **music mood** (e.g. tense, somber, bright, quiet) — including segments that must be music-quiet or music-free. **Tool/command:** `muse.write`/`muse.edit` on the script file. **Intermediate output:** the locked script marker `[KỊCH BẢN CHỐT]`; from this gate on, the narration text is frozen — any wording change goes through step 11. **Error branch:** a segment with no music mood recorded blocks the gate: G4b cannot lock music for a segment whose mood is unknown.
4. **G3 — Voice and SRT. Action:** FIRST verify the voice against the project type, then generate: **Each project type uses exactly one locked voice — for YouTube videos, the single Minimax voice locked for the channel in its series bible, generated via ai33.pro v3 (provider `minimax`)**. Use your own voice: create or clone your voice in your own ai33.pro account and record its `voice_id` in the series bible — never publish a personal voice ID in a shared or public document. A voice reserved for another project type (for example the news-bulletin voice) must **never** be used for a YouTube video (see section 2 for the incident this rule comes from). Generate the voice from the locked script, listen to the head, middle and end, then export the SRT from the actual voice recording — the SRT is the locked time-map for everything after this gate. Run a light loudness check here (RMS even from head to tail) so a dead or dipping voice segment surfaces now, not at G6. **Run the subtitle gate:** scan the SRT with the **srt-unverified-marker-gate** skill; any unverified marker blocks this gate until resolved there. **Tool/command:** ai33.pro v3 — `POST https://api.ai33.pro/v3/text-to-speech` (header `xi-api-key`, body with `provider` and `voice_id` as above), poll `GET /v3/task/{task_id}` for `data.status` and `data.metadata.audio_url`; download the audio with a browser User-Agent. RMS spot check via `muse.exec`: `ffmpeg -i voice.wav -af astats=metadata=1 -f null -` on head/middle/end excerpts. **Intermediate output:** voice file + SRT file, both present and checked — until both exist, no music is chosen and no visual is made. **Handoff rule for partner videos:** the partner receives the voice FILE + the SRT file only. An API key is never shared, pasted into the sheet, or written into any project file. **Error branch:** wrong voice ID discovered after generation → discard the voice entirely and regenerate; never "fix" a wrong voice in the mix. A marker found by the subtitle gate → resolve it under that skill before proceeding.
5. **G4a — Visual direction. Action:** Choose the engine/style from the series' assignment table; write TREATMENT.md with 3 candidate structures, the 1 chosen and why, and a mapping table segment → visual type/effect. **Tool/command:** `muse.write` on TREATMENT.md in the project folder. **Intermediate output:** TREATMENT.md visual half. **Error branch:** a style with no approved sample anywhere in the series triggers the sample-segment requirement at step 7 — it cannot be waived at G5.
6. **G4b — Music gate. Action:** Lock one track per segment from the music moods recorded at G2, into the same TREATMENT.md mapping table. Every track entry records: name, source, **license**, duration, and in/out points — the in/out points are placed at segment boundaries read off the locked SRT timestamps. Set the music level relative to the voice (ducking) to the series' standard value and record it in the manifest (step 7). **Only music with a clear license is used** — a track without a verifiable license is rejected at this gate, however well it fits. **Tool/command:** `muse.edit` on TREATMENT.md; license evidence filed in the project folder. **Intermediate output:** TREATMENT.md music half + the per-series ducking value. **Error branch:** no licensed track fits a segment's mood → the segment is marked deliberately music-free in the table (a planned silence, which G6 accepts); an unlicensed track is never the fallback.
7. **G4c — Technical lock and smoke test; G4d — sample segment. Action:** Write MANIFEST.md: tool versions, render configuration (resolution, fps), the per-series ducking value from step 6, and the asset list (visuals, voice, SRT, music). Then run a **smoke test of 10–15 seconds for every stage that will be used** (drawing effect, music mix, subtitle burn-in) using the exact tools and configuration of the real render. Build the style frame/storyboard and compare it against the series standard. Nothing in the manifest changes mid-production; changing a tool or configuration means returning to this gate. If the style or pipeline is new, also produce a **sample segment of 60–90 seconds** with voice, music and subtitles complete, and wait for the channel owner's `[SAMPLE APPROVED]` before G5; a style with an approved sample needs no new sample. **Tool/command:** `muse.write` on MANIFEST.md; smoke renders via the project pipeline through `muse.exec`. **Intermediate output:** MANIFEST.md + passing smoke tests (+ approved sample, when required). **Error branch:** any failure in a smoke test is fixed here, at 15 seconds of cost — never carried into G5 "to see how it goes".
8. **G5 — Production. Action:** Build exactly the storyboard and timeline locked at G4, inside the single timeline. Source real material into the project folder with its source recorded. **Render segment by segment**; each segment gets a fast self-check (picture present, voice present, music present per plan, drawing effect runs to completion) before it is joined. Long films render in parts of ≤ ~400 seconds and are then joined, with the total frame count reconciled after joining. **Tool/command:** the project pipeline via `muse.exec`; per-segment checks with `ffprobe` and frame grabs. **Intermediate output:** checked segments + joined master candidate. **Error branch:** do not improvise a new style, structure, wording or track mid-render — a problem belonging to an earlier gate stops production and goes back to that gate (step 11).
9. **G6 — Mandatory QA (no pass, no delivery). Action:** Run the full check battery on the master candidate and record every measurement in SAN-XUAT.md (written at step 10, started here):
   - *Picture/technical:* `ffprobe` — resolution and fps match the manifest, duration matches the timeline within **±0.5 s**, audio stream present. `blackdetect` clean (`ffmpeg -i master.mp4 -vf blackdetect=d=0.5:pix_th=0.10 -an -f null -`); intentionally dark frames are inspected by eye to separate design from defects. Contact sheet every 15 seconds plus a frame at every segment boundary — all viewed. Drawing effects run to completion in every segment (whiteboard: frozen-after-drawing ≤ ~2.5 s per segment; documentary: no static image longer than ~25 s; a pattern interrupt every 20–30 s).
   - *Figures:* every number shown on screen is cross-checked against the G0 fact table — **100% of figures; one wrong figure means that segment is redone**.
   - *Audio:* integrated loudness **−14 ±1 LUFS**, true peak **≤ −1 dB** (`ffmpeg -i master.mp4 -af loudnorm=print_format=json -f null -`). **RMS scan in 10-second windows across the whole film**: voice level even from head to tail, and music present in every segment where the G4b plan says it should be — no unplanned silent gap (planned music-free segments from step 6 are the only exception). Music never covers the voice; ducking even; no abrupt cut or obvious loop at track transitions. Listen to head, middle and end on speakers and on headphones. The music actually used matches the license table from G4b, track for track.
   - *Subtitles:* cue count complete against the SRT from G3, text matches the narration at head, middle and end — and the **subtitle gate is run again**: the final SRT passes the **srt-unverified-marker-gate** skill with 0 markers remaining.
   **Tool/command:** `muse.exec` with the commands above; contact sheet via `ffmpeg -i master.mp4 -vf "fps=1/15,scale=320:-1,tile=6x5" contact_sheet_%03d.png` — one sheet per 30 frames (450 s of film); a longer film yields numbered sheets, and every sheet is viewed. **Intermediate output:** the measurement set for SAN-XUAT.md. **Error branch:** any failed item names its segment and its gate of origin; the fix happens at that gate (step 11), and G6 is re-run on the affected scope — a failed G6 is never overridden by deadline pressure.
10. **G7 — Delivery and posting. Action:** Deliver the package: **master + SRT + thumbnail + SAN-XUAT.md** (method, QA measurements, music list with licenses, and the remaining weaknesses — recorded honestly). Posting happens only after the channel owner approves the master; the default visibility is **Private** unless the owner orders otherwise. Feedback after delivery opens exactly one new fix round, limited to the points raised. **Tool/command:** `muse.write` on SAN-XUAT.md; upload through the channel's normal posting route after approval. **Intermediate output:** delivered package + posting state. **Error branch:** no approval, no posting — a finished master sitting unposted is a valid end state; an unapproved public post is not.
11. **Changes and errors (applies at every gate). Action:** When anything changes, redo exactly what the propagation table says — no more, no less. **Tool/command:** the tools of the gate the change returns to (see that gate's step above). **Intermediate output:** the redone artefact at that gate.

    | Change | Redo |
    |---|---|
    | Narration wording | That segment's voice + SRT → re-check that segment's music and visuals |
    | Voice speed/length | SRT → that segment's music and visual timing |
    | Music track | Only the track and ducking; picture and voice untouched |
    | Visual style | Back to G4: new smoke test and new sample segment |
    | Effect bug in one segment | Re-render that segment only; if it repeats across segments, back to G4c to re-check tool and configuration |

    **Error rule:** an error surfacing at a gate is fixed at that gate, never passed downstream. **The same error twice with the same cause: stop**, find the root cause, record it in SAN-XUAT.md, and add a new checkpoint to this workflow — do not patch a new layer onto a broken build; return to the nearest decision gate. **Hot-news shortcut:** G4 may be shortened (the sample segment may be skipped when reusing an approved style and music set), but G2, G3, the G4c smoke test and G6 are never skipped.

## 6. Output & acceptance thresholds

A video may be delivered at G7 only when ALL of these measurable conditions hold:

- Gate order respected: script locked (G2) before voice (G3), SRT before music lock (G4b) and before visuals (G5); one single timeline containing picture, voice, music and subtitles.
- Voice: the locked voice ID for the project type (per the series bible) on 100% of the video; any other project's voice appears in **0** videos of this type.
- Loudness: integrated **−14 ±1 LUFS**; true peak **≤ −1 dB**.
- RMS 10-second-window scan: voice present and even in 100% of windows; music present in 100% of segments where the G4b plan requires it (planned music-free segments excluded, and they are listed as planned).
- Figures on screen vs the G0 fact table: **100%** cross-checked, **0** mismatches.
- Duration vs timeline: within **±0.5 s**; resolution and fps exactly as locked in MANIFEST.md; `blackdetect` defects: **0** unexplained.
- Subtitles: cue count equal to the G3 SRT; final SRT passes **srt-unverified-marker-gate** with **0** markers remaining.
- Music: 100% of tracks in the master appear in the G4b table with a recorded license; unlicensed tracks: **0**.
- Process artefacts present: fact table (G0), `[DÀN BÀI CHỐT]`, `[KỊCH BẢN CHỐT]` with a music mood on 100% of segments, TREATMENT.md, MANIFEST.md, smoke-test results for 100% of pipeline stages used, sample approval when the style/pipeline was new.
- Delivery package complete: master + SRT + thumbnail + SAN-XUAT.md with the QA measurements and the music/license list; posting state = not posted, or posted Private, or posted as the channel owner explicitly ordered after approval.

## 7. Examples

- **Correct example:** "Start the new explainer episode: why do gym chains give away free pizza?" → Run the gates: G0 fact table with each figure sourced; G1 outline locked; G2 script locked with a music mood on every segment; G3 voice generated with the channel's locked Minimax voice, SRT exported from the recording, RMS spot check even, subtitle gate clean; G4 treatment + licensed tracks locked at SRT boundaries, manifest written, smoke tests pass, 75-second sample approved; G5 rendered segment by segment; G6 measures −14.2 LUFS, true peak −1.3 dB, RMS scan clean, all figures match the fact table; G7 package delivered and posted Private after the owner's approval.
- **Boundary example:** "The finished video is ready, but the delivered SRT still contains a `[CHƯA KIỂM CHỨNG …]` note — clean it and post." → Do NOT resolve the marker inside this workflow and do NOT post: the marker belongs to the **srt-unverified-marker-gate** skill, which this workflow invokes at G3 and G6. Route the file through that gate (scan, verify or remove the claim, re-scan to 0 markers, cue count unchanged); only when the gate passes does the package return here for G7 delivery.

## 8. Common failures & fixes

- **Music missing in some segments.** Cause: music glued on after rendering, or segments whose mood was never recorded. Fix: music is locked per segment at G4b from the G2 mood column, inside the single timeline; the G6 RMS window scan names the exact silent segment.
- **Wrong voice on a YouTube video.** Using the news-bulletin voice for a YouTube explainer (the YPP v1 incident) forced a full re-record. Fix: verify the voice ID at G3 before generating; a wrong voice is discarded, never mixed around.
- **Effect errors discovered only in the full render.** Cause: no smoke test, or tool configuration drifted between runs. Fix: G4c smoke test (10–15 s per stage) with versions and configuration locked in MANIFEST.md; a mid-production configuration change returns the project to G4.
- **A small script edit desyncs everything.** Fix: apply the propagation table (step 11) — a wording change redoes that segment's voice + SRT and re-checks its music/visual timing; nothing outside the table is touched.
- **Subtitles or music "patched" after the render.** This breaks the single-timeline rule and every measurement taken at G6. Fix: any such change re-enters at its own gate and the affected G6 checks are re-run.
- **Unlicensed track slipped in for fit.** Fix: license is a G4b entry requirement recorded in TREATMENT.md; G6 reconciles the used-music list against it track for track.
- **Posting before approval, or public by default.** Fix: G7 — no approval, no posting; default visibility is Private.
- **Partner asking for the API key "to generate the voice themselves".** Fix: refuse and restate the handoff rule — the partner receives the voice file + SRT only; keys are never shared (section 11).

## 9. Verify before reporting done

- [ ] Gate markers on record: brief + fact table approved, `[DÀN BÀI CHỐT]`, `[KỊCH BẢN CHỐT]` with music mood on every segment.
- [ ] Voice ID verified against the series bible's locked voice for the project type; voice file + SRT both delivered from G3.
- [ ] TREATMENT.md holds the visual mapping and the per-segment music table with a license for every track; MANIFEST.md matches the tools/configuration actually used.
- [ ] Smoke tests passed for every pipeline stage; sample segment approved when the style/pipeline was new.
- [ ] G6 measurements recorded in SAN-XUAT.md: LUFS and true peak inside thresholds, RMS 10 s scan clean, duration within ±0.5 s, blackdetect clean, 100% of figures matched to the G0 table.
- [ ] Final SRT passed srt-unverified-marker-gate with 0 markers remaining and the G3 cue count.
- [ ] Delivery package complete: master + SRT + thumbnail + SAN-XUAT.md (including remaining weaknesses, honestly recorded).
- [ ] Posting state matches the channel owner's decision: approved before posting, Private by default.

## 10. Dependencies & handoff

- **Inputs from:** the channel owner's brief/backlog (G0–G1), the series bible (style and music assignment), the ai33.pro v3 text-to-speech API (voice at G3; get access via the referral sign-up link in section 4), and the main production repo's pipeline (rendering, G5).
- **Outputs to:** the channel owner (delivery package at G7 for approval and posting) and, mid-pipeline, the editing partner — who receives the locked outline/script via the shared channel sheet and, after G3, the voice file + SRT file only.
- **Companion skills:** **srt-unverified-marker-gate** (on this repo) is invoked at G3 and again at G6 — no SRT leaves either gate with an unverified marker in it. Engine-specific production documents (per-series bibles, the documentary pipeline document, the partner channel's standing instructions) sit *under* this gate framework; where they conflict with a gate, the gate wins.
- **Source:** rewritten for Muse from the channel's internal pre-edit workflow v2 (issued 2026-10-26), which this skill supersedes as the portable form; the internal document remains the historical record of the incidents in section 2.

## 11. Guardrails

- Never include secrets, passwords, or API keys in files, messages, sheets, or examples — and never share the text-to-speech API key with an editing partner; partners receive voice + SRT files only.
- Never hide actions from the user or bypass their approvals: no posting before the channel owner approves the master, and never public by default.
- Confirm with the user before any irreversible or outward-facing action.
- Never present an opinion or assumption from the G0 fact table as a sourced fact, in the script, on screen, or in a report.
- Never let deadline pressure skip G2, G3, the G4c smoke test, or G6 — the hot-news shortcut (step 11) is the only sanctioned abbreviation, and it skips none of those.
