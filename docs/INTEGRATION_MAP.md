# Integration map — AdityaRaj2434/awesome-muse-ai-skills

Boss Tài approved integrating this upstream list on 2026-10-28 (confirmed in main chat, relayed to the project channel). The upstream repo is **a curated list only** (README + MIT LICENSE, no code): 30 skills written for other agents. Nothing is copied wholesale — every entry is **rewritten to this repo's v2 template for Meta Muse**, with upstream attribution and license noted in each skill's "Dependencies & handoff" section.

Star counts in the upstream README are the upstream repos' self-reported numbers; they are not republished here as verified facts.

## Mapping to this repo's categories

### Content & media (10) — target folder: `skills/content-and-media/`

| Upstream entry | Upstream source | Wave |
| --- | --- | --- |
| hyperframes | heygen-com/hyperframes | 1 — deep research first |
| openfilm | openfilm/openfilm | 1 — deep research first |
| animated-short-film | sonlovinbot/Muse-animated-short-film | 1 — deep research first |
| video-use | browser-use/video-use | 2 |
| ai-video-director | custom — no public upstream | 2 — write new from pattern |
| media-revision-delivery | local — no public repo | 2 — write new from pattern |
| ai-video-editor | MartinDelophy/ai-video-editor | 3 |
| voicestudio | debpalash/VoiceStudio | 3 |
| humanizer | blader/humanizer | 4 |
| outward_identity_discipline | local — no public repo | **Excluded** (see below) |

### Coding & development (16) — target folder: `skills/coding-and-development/`

| Upstream entry | Upstream source | Wave |
| --- | --- | --- |
| scroll-craft | nateherkai/scroll-craft | 4 |
| hallmark | Nutlope/hallmark | 4 |
| threeui | MengTo/threeui | 4 |
| diagram-design | cathrynlavery/diagram-design | 4 |
| emilkowalski-skills | emilkowalski/skills (15 sub-skills) | 4 |
| anatomy-isometric | wheresryan22/anatomy | 4 |
| agent-skills | addyosmani/agent-skills (25 workflows) | 4 |
| playwright-cli | microsoft/playwright-cli | 4 |
| reverse_engineer_anything | morluto/rea | 4 |
| android-skills | android/skills | 4 |
| headroom | headroomlabs-ai/headroom | 4 |
| mattpocock-skills | mattpocock/skills | 4 |
| security-audit | cloudflare/security-audit-skill | 4 |
| ponytail | DietrichGebert/ponytail | 4 |
| openshell | NVIDIA/OpenShell | 4 |
| graphify | Graphify-Labs/graphify | 4 |
| paperclip | paperclipai/paperclip | 4 (multi-agent orchestration) |

### Data & research (1) — target folder: `skills/data-and-research/`

| Upstream entry | Upstream source | Wave |
| --- | --- | --- |
| agent-reach | Panniantong/Agent-Reach | 4 |

### Productivity & office work (2) — target folder: `skills/productivity/`

| Upstream entry | Upstream source | Wave |
| --- | --- | --- |
| ai-job-search | MadsLorentzen/ai-job-search | 4 |
| apply_sweep_checkpointing | local — no public repo | 4 — write new from pattern |

## Rewrite waves

- **Wave 0 (pilot, already running):** `srt_unverified_marker_gate` — Misa's own installed video skill, rewritten first to prove the v2 template end-to-end (draft → review against REVIEW_CHECKLIST → test-load into Muse).
- **Wave 1:** hyperframes, openfilm, animated-short-film — deep research before writing.
- **Wave 2:** video-use; ai-video-director and media-revision-delivery written new from their patterns.
- **Wave 3:** ai-video-editor, voicestudio.
- **Wave 4:** everything else, one category at a time, only after the video waves prove the template.

## Excluded

- **outward_identity_discipline** — its core rule (outward messages must never reveal an assistant/AI is involved) conflicts with this repo's guardrails: skills must not instruct Muse to hide its actions or identity from people it interacts with. Not integrated as-is; reconsider only if rewritten around transparent disclosure.

## Roles

Misa drafts and critiques; Muse standardises against the v2 template, test-loads each skill into Muse to verify it triggers and reads correctly, and pushes to this repo. Folder layout: `skills/<category-slug>/<skill-name>/` — each entry lands in the folder of the category section it is listed under (registry: [CATEGORIES.md](CATEGORIES.md)), one primary category per skill.
