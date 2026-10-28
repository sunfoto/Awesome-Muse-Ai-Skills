# Skills

Every folder here is one installable Muse skill: copy the folder into your Muse custom skills directory (`~/workspace/custom-skills/`) and start a new conversation.

| Skill | What it does |
| --- | --- |
| [example-hello-muse](example-hello-muse/SKILL.md) | Minimal working example — verify your install, then copy the pattern. |
| [srt-unverified-marker-gate](srt-unverified-marker-gate/SKILL.md) | Gate a delivered video's SRT subtitle file before QA handover or posting: full-text scan for unverified-claim markers, resolve each against a real source, block delivery while any marker remains. |
| [pre-edit-workflow-g0-g7](pre-edit-workflow-g0-g7/SKILL.md) | Run a gated pre-edit workflow (gates G0–G7) for narrated videos: fact-check the draft and build the fact table, lock the script, then record voice, SRT, music and visuals in that fixed order on a single timeline, with a numeric QA gate before delivery. |

Want to add one? Start from [../templates/skill-template/](../templates/skill-template/) and read [../CONTRIBUTING.md](../CONTRIBUTING.md).
