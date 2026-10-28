# Skills

Skills here are organised **by domain**: every skill lives in exactly one category folder, at `skills/<category-slug>/<skill-name>/`. Copy a skill's folder into your Muse custom skills directory (`~/workspace/custom-skills/`) and start a new conversation to install it.

The four domain categories (authoritative registry: [../docs/CATEGORIES.md](../docs/CATEGORIES.md)):

| Category | Folder |
| --- | --- |
| Content & media | `content-and-media/` |
| Coding & development | `coding-and-development/` |
| Data & research | `data-and-research/` |
| Productivity | `productivity/` |

A category folder is created when its first skill lands. Every new or updated skill must be filed under its correct domain folder and listed in the registry — see the classification rule in [../docs/CATEGORIES.md](../docs/CATEGORIES.md).

## Content & media

| Skill | What it does |
| --- | --- |
| [srt-unverified-marker-gate](content-and-media/srt-unverified-marker-gate/SKILL.md) | Gate a delivered video's SRT subtitle file before QA handover or posting: full-text scan for unverified-claim markers, resolve each against a real source, block delivery while any marker remains. |
| [pre-edit-workflow-g0-g7](content-and-media/pre-edit-workflow-g0-g7/SKILL.md) | Run a gated pre-edit workflow (gates G0–G7) for narrated videos: fact-check the draft and build the fact table, lock the script, then record voice, SRT, music and visuals in that fixed order on a single timeline, with a numeric QA gate before delivery. |

## Coding & development · Data & research · Productivity

Defined categories with no skills yet — the first entries arrive per the [integration roadmap](../docs/INTEGRATION_MAP.md), and their folders are created at that point.

## Format example (unclassified)

[example-hello-muse](example-hello-muse/SKILL.md) — a minimal working example that stays at the top level of `skills/` on purpose: it demonstrates the `SKILL.md` format and is **not** a classified skill, so it does not live in a domain folder. Verify your install with it, then copy the pattern.

Want to add a skill? Choose its category from [../docs/CATEGORIES.md](../docs/CATEGORIES.md) first, start from [../templates/skill-template/](../templates/skill-template/), and read [../CONTRIBUTING.md](../CONTRIBUTING.md).
