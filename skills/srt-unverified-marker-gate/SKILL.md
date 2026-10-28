---
name: srt-unverified-marker-gate
description: Gate a delivered video's SRT subtitle file before QA handover or posting — scan the full text for unverified-claim markers (e.g. "CHƯA KIỂM CHỨNG"), resolve every marker against a real source, and block delivery while any marker remains.
---

# SRT Unverified Marker Gate

## 1. Context

Video producers sometimes leave editorial flags inside a delivered subtitle file — bracketed notes marking a claim as not yet fact-checked, most often the Vietnamese marker `[CHƯA KIỂM CHỨNG …]` ("not yet verified"). If such a file ships, the flag itself can end up published on screen or in the platform's subtitle track, and the unverified claim goes out as if it were fact. This skill is the checkpoint between a finished video package and QA handover or posting: it scans the delivered `.srt`, forces every marker to a resolution, and blocks the delivery until the file is clean. It revolves around plain text tools (`grep` via `muse.exec`, `muse.read`) and web verification (`browser.search`, `browser.open`).

## 2. Why the hard rules exist

In production (a 109-cue documentary subtitle file), three markers survived all the way to the upload step: they were only caught because the full file text was searched before posting. Two claims turned out to be verifiable in minutes against mainstream sources (a wire-service report and national statistics); the third needed its wording corrected. Had the file been spot-checked visually instead of searched as text, at least one marker would likely have been published. Hence the non-negotiable rules below: full-text scan, never sampling; zero markers remaining, no exceptions for "minor" ones; re-scan the whole file after every fix.

## 3. When to use / when NOT to use

- Use when: a video package (master + SRT) is delivered for QA, handover, or posting, and the subtitle file may contain editorial or unverified-claim markers.
- Use when: a producer reports they "cleaned" a subtitle file and the cleanup needs independent confirmation before posting.
- Do NOT use when: the task is fact-checking a script *before* voice/subtitle production — that belongs to the pre-edit fact-table step of the video production process (the G0–G7 pre-edit workflow; its skill draft is next in queue and has no skill name yet, so this draft names the destination as "G0–G7 pre-edit workflow, fact-table step" and the skill name will be filled in when that draft exists), not this gate.
- Do NOT use when: the task is subtitle timing, formatting, translation, or styling with no verification flags involved — route it to the video production/editing skill handling the project.

## 4. Inputs required

- The delivered SRT file itself (path in the delivery package). A video file alone is not enough — if no `.srt` was delivered, stop and request it from the producer.
- The delivered cue count, or the means to count cues from the delivered file before any edit (step 2 of the procedure records it).
- Access to a search/browse tool for source verification. If no source can be reached, every flagged claim is treated as unverifiable (see step 5) — never resolved from memory.

## 5. Procedure

1. **Action:** Locate the delivered `.srt` in the delivery package and record its path and byte size. **Tool/command:** `muse.exec` with `ls -la <package-dir>` and `muse.read` on the file header. **Intermediate output:** confirmed file path; the pristine delivered copy stays untouched — all edits happen on a working copy named `<basename>.gate.srt` in the same delivery folder (for example, delivered `XD-08.srt` → working copy `XD-08.gate.srt`). Record both paths at this step, so step 7 can never hand over the wrong file. **Error branch:** if the package has a video but no `.srt`, stop, tell the producer the subtitle file is missing, and do not start the gate.
2. **Action:** Scan the ENTIRE file text for markers and record the delivered cue count. **Tool/command:** `muse.exec`:
   `grep -n -i -E "CHƯA[[:space:]]+KIỂM[[:space:]]+CHỨNG|CHUA[[:space:]]+KIEM[[:space:]]+CHUNG|CHƯA[[:space:]]+XÁC[[:space:]]+MINH|CHUA[[:space:]]+XAC[[:space:]]+MINH|CHƯA[[:space:]]+XÁC[[:space:]]+THỰC|CHUA[[:space:]]+XAC[[:space:]]+THUC|CẦN[[:space:]]+KIỂM[[:space:]]+CHỨNG|CAN[[:space:]]+KIEM[[:space:]]+CHUNG|NOT[[:space:]]+VERIFIED|UNVERIFIED|TODO|TBD" <file.srt>` and, for any bracketed editorial note, `grep -n "\[" <file.srt>` reviewed by eye; cue count via `grep -cE "^[0-9]+$" <file.srt>`. The `-i` flag already covers lowercase variants; `[[:space:]]+` covers multi-space / tab variants, so a marker split by extra whitespace is still caught. This pattern list is a MINIMUM: if a new verification-wording variant is met during a scan, add it to the pattern and re-scan the whole file from this step before proceeding. Every bracketed note found by the second grep is classified in the resolution table as either (i) a marker — it contains verification language or a factual claim — or (ii) a non-marker production note (e.g. `[music]`, `[pause]`, a stage direction with no factual claim), logged with its reason and excluded from the found/resolved counts; a non-marker never blocks the gate, but it must be listed, never silently skipped. A bare (unbracketed) `TODO` / `TBD` / `UNVERIFIED` hit goes through the SAME classification: it is a marker only if its context is verification language or a factual claim; an ordinary word in dialogue (for example a character saying "to do") is logged as a non-marker with its reason, excluded from the counts, and never auto-counted as a blocking marker — and never silently skipped either. **Intermediate output:** a hit list with line number, cue number, timestamp, and the exact flagged claim for every hit, plus the baseline cue count. **Error branch:** if `grep` is unavailable, read the whole file in chunks with `muse.read` and scan manually — sampling or spot-checking is never an acceptable substitute.
3. **Action:** Build a resolution table with one row per hit: cue, timestamp, claim as written, and an empty "source / decision" column. **Tool/command:** working notes in the QA file for the project. **Intermediate output:** the table that steps 4–5 fill in; nothing is resolved from memory or from this table alone. **Error branch:** if a marker's claim is ambiguous (the bracket does not say what is unverified), ask the producer what needs checking before proceeding — do not guess the claim.
4. **Action:** Verify each flagged claim against a primary or mainstream source (official statistics, wire services, the institution's own publication). **Tool/command:** `browser.search` to find the source, `browser.open` to read it; record the source name and the exact figure/date it supports next to the claim. **Intermediate output:** per-claim verdict — *verified* (with source) or *unverifiable*. **Error branch:** if the source page cannot be opened or contradicts the claim, the claim is *unverifiable* — never mark a claim verified on a search snippet alone when the number itself matters.
5. **Action:** Resolve every marker in the working copy. Verified claim → delete only the marker, keep the claim text exactly as written. Unverifiable claim → delete the claim text together with its marker, keeping the cue's timing and position intact (shorten the cue text; never delete the whole cue) — but only if some non-empty cue text remains. If deleting the claim would empty the cue entirely, that is NOT a gate fix: an empty-text cue is not an acceptable passed file, so return the file to the producer with the exact cue number, timestamp, and claim text, exactly as for a whole-cue removal below. If the producer insists the whole cue (timing segment included) must go, that is also NOT a gate resolution: return the file to the producer, who re-delivers a new SRT, and the gate restarts from step 1 with the new file's cue count as the new baseline; the QA report records the re-delivery, the old baseline, and the new baseline separately, never silently. Claim you cannot judge either way → leave it untouched and return it to the producer with the exact cue number, timestamp, and claim text. **Tool/command:** `muse.edit` for surgical per-cue edits. **Intermediate output:** cleaned working SRT + filled resolution table. **Error branch:** if an edit would change the cue count, shift timestamps, or leave a cue with empty text, undo it and either re-apply the fix inside the existing cue (when non-empty text remains) or return the file to the producer as above.
6. **Action:** Re-scan the WHOLE working file (repeat step 2 verbatim) and compare cue counts. **Tool/command:** same `grep` commands as step 2. **Intermediate output:** hit count after fixes and cue count after fixes. **Error branch:** if any hit remains, go back to step 4 for that hit — the gate is not passed; if the cue count differs from the baseline, restore the working copy from the delivered file and redo step 5 more carefully.
7. **Action:** Write the gate result into the project's QA report and hand over. **Tool/command:** `muse.write`/`muse.edit` on the QA report. **QA report location and format:** the QA report is a Markdown file in the same delivery folder, named `<basename>.QA.md` (for example, `XD-08.QA.md` next to `XD-08.srt`) — if the project already has a QA file, append to that existing file instead of creating a second one, and record which file was used. The gate entry in it is a Markdown section titled "SRT Unverified Marker Gate" containing: the three numbers (found / resolved / remaining), the resolution table (cue, timestamp, claim, source or deletion), the handover file name (must be the re-scanned `<basename>.gate.srt` working copy from step 1, never the delivered original), and, in a re-delivery case, the old baseline, the new baseline, and the re-delivery reason. **Intermediate output:** QA entry as above plus the handover file path. **Error branch:** if remaining > 0, state the blocker explicitly with its owner (producer or checker) and STOP — no QA handover, no posting, no "post first, fix later."

## 6. Output & acceptance thresholds

The gate passes only when ALL of these measurable conditions hold:

- Full-text scan coverage: 100% of the file (every line), on the final version.
- Markers remaining: **0**.
- Cue count after fixes: exactly equal to the delivered cue count (difference = 0). Cue slots are retained only when non-empty cue text remains after a claim is deleted (step 5); an empty-text cue is never a pass, and a whole-cue removal is a producer re-delivery with a new baseline, not a gate fix, and must be recorded as such.
- Handover file: the re-scanned `<basename>.gate.srt` working copy (step 1), not the delivered original; QA report path and format as defined in step 7.
- Non-marker bracketed notes are listed with their classification reason and excluded from the found/resolved counts; they never block the gate.
- Every resolved marker has a recorded outcome: a named source for kept claims, or a deletion for unverifiable ones — 0 resolutions "from memory".
- QA report contains the three numbers (found / resolved / remaining) and, if remaining > 0, the blocking cue, claim, and owner.

## 7. Examples

- **Correct example:** "Here is the finished video package, master + SRT, ready for QA before we post it privately." → Run the gate: scan finds 3 bracketed `[CHƯA KIỂM CHỨNG …]` markers in a 109-cue file; two claims verify against a wire report and national statistics (markers deleted, claims kept), one claim cannot be verified and is removed with its marker; re-scan shows 0 hits and the cue count is still 109; QA report records 3 found / 3 resolved / 0 remaining and the package proceeds.
- **Boundary example:** "Please check the facts in this draft script before we record the voiceover." → Do NOT use this gate: there is no delivered SRT yet, and pre-production fact-checking belongs to the pre-edit workflow's fact-table step (destination named here as "G0–G7 pre-edit workflow, fact-table step" — no skill name exists yet; the name will be filled in when the G0–G7 draft is written, which is the next draft in queue). Route it there; this gate only applies to markers inside a delivered subtitle file.

## 8. Common failures & fixes

- **Spot-checking instead of searching.** Watching the video or reading a few cues misses markers. Fix: always run the full-text scan (step 2); treat any visual pass as a supplement only.
- **Missing an accent, case, or spacing variant.** The marker appears as `CHƯA KIỂM CHỨNG`, unaccented `CHUA KIEM CHUNG`, lowercase, or reworded (`CHƯA XÁC MINH`), possibly with extra spaces/tabs. Fix: the scan pattern uses `-i` plus `[[:space:]]+` and covers accented and unaccented variants, plus a manual review of every bracketed note, each classified as marker or non-marker production note (step 2).
- **Deleting the marker but keeping an unverified claim** (or the reverse). Fix: marker and claim share one verdict from step 4 — verified keeps the claim and drops the marker; unverifiable drops both.
- **Cue count drifts after edits.** A deleted cue breaks timing for everything after it. Fix: edits stay inside the cue; step 6 compares counts and forces a redo on any difference.
- **Leaving an empty cue after deleting a claim.** An empty-text cue is not a valid passed file and must not be handed over as "cleaned". Fix: return that cue to the producer for re-delivery (step 5); the gate restarts from step 1 with the new baseline.
- **Handing over the wrong file.** The delivered original and the cleaned working copy sit in the same folder. Fix: the working copy is always `<basename>.gate.srt` (step 1) and step 7 names that exact file; handing over the original is a fail.
- **Reporting "clean" without numbers.** Fix: the QA entry must carry found / resolved / remaining counts (section 6); a bare "đã kiểm tra" is not a pass.

## 9. Verify before reporting done

- [ ] Full-text scan re-run on the final file: 0 marker hits.
- [ ] Cue count of the final file equals the delivered cue count.
- [ ] Resolution table complete: every hit has a source or a deletion on record.
- [ ] QA report updated with found / resolved / remaining and the handover decision (pass, or blocked + owner); QA report path recorded (step 7 format: `<basename>.QA.md` in the delivery folder, or the project's existing QA file).
- [ ] The file handed over is the re-scanned `<basename>.gate.srt` working copy, not the delivered original and not the pre-fix version.
- [ ] No cue in the handed-over file has empty text.

## 10. Dependencies & handoff

- **Inputs from:** the video producer's delivery package (master + SRT + QA notes), produced under the project's pre-edit/production workflow.
- **Outputs to:** the QA/posting step — the cleaned SRT, the QA gate entry, and an explicit pass/block decision. A blocked gate returns the specific cue + claim to the producer for verification.
- **Companion practices:** keeping unverified claims in a separate producer checklist (instead of markers inside the SRT) prevents most gate hits; a per-video verified-facts list lets repeat claims resolve in step 4 without a new search.
- No connector or account access is required; only local file tools and web search/browse.

## 11. Guardrails

- Never include secrets, passwords, or API keys in files, messages, or examples.
- Never hide actions from the user or bypass their approvals.
- Confirm with the user before any irreversible or outward-facing action.
- Never mark a claim "verified" without a named source you actually opened; absence of evidence resolves to *unverifiable*, not to *probably true*.
- Never let a delivery pass this gate with a marker remaining, whatever the deadline pressure — surface the blocker and its owner instead.
