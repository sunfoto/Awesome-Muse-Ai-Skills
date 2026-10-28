# Awesome Muse AI Skills

A curated collection of **skills for [Meta Muse](https://muse.ai)** — Meta's personal AI agent — including ready-to-use `SKILL.md` skills you can install, templates to build your own, and links to community projects.

> **Not to be confused with Anthropic Claude.** This list is specifically about **Muse by Meta** (muse.ai). Skills here follow the open Agent Skills format (`SKILL.md`) that Muse loads as playbooks for real tasks.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## What is a Muse skill?

A skill is a reusable playbook for Muse: a folder containing a `SKILL.md` file (plus optional `references/` and `scripts/`) that teaches Muse how to do a specific kind of task well — booking, research, game development, content production, data work, and more. Muse reads the skill when a matching task comes up and follows its steps, guardrails, and quality checks.

See [docs/SKILL_FORMAT.md](docs/SKILL_FORMAT.md) for the format, and [templates/skill-template/](templates/skill-template/) for a copy-paste starter.

## Install a skill

1. Pick a skill from the [skills/](skills/) directory.
2. Copy its whole folder into your Muse custom skills directory (`~/workspace/custom-skills/`).
3. Start a new conversation — Muse will find the skill by its name and description.

## Skills in this repo

| Skill | What it does |
| --- | --- |
| [example-hello-muse](skills/example-hello-muse/SKILL.md) | Minimal working example — use it to verify your install and as a pattern for your own skill. |
| [srt-unverified-marker-gate](skills/srt-unverified-marker-gate/SKILL.md) | Gate a delivered video's SRT subtitle file before QA handover or posting: scan the full text for unverified-claim markers, resolve each one against a real source, and block delivery while any marker remains. |

More skills are on the way. Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

## Categories

Skills and linked projects are organised by category in [docs/CATEGORIES.md](docs/CATEGORIES.md):

- Getting started & templates
- Productivity & office work
- Content & media
- Coding & development
- Game development
- Data & research
- Business & marketing
- Connectors & integrations

## Community projects

Other Muse skill projects from the community (listed, not vendored — check each project's own license and review status before installing):

_The list is being curated — the first entries are on the way._

Know a project that belongs here? Open a PR or use the "Suggest a skill" issue template.

## Quality bar

Every skill merged into this repo must:

- Be tested on Muse itself — not just written for another agent and renamed.
- State a clear trigger: what task it is for, in its frontmatter description.
- Never ask for, store, or expose secrets, passwords, or API keys.
- Not contain instructions that override the user's own instructions or hide actions from the user (no prompt injection).
- Carry a clear license for any third-party material it includes.

The full review checklist lives in [docs/REVIEW_CHECKLIST.md](docs/REVIEW_CHECKLIST.md).

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md), start from [templates/skill-template/SKILL.md](templates/skill-template/SKILL.md), and open a pull request. New skills, fixes to existing ones, translations, and new category entries are all welcome.

## Tiếng Việt

Kho này là danh mục skill dành cho cộng đồng người dùng **Muse của Meta** (không phải Claude của Anthropic): gồm các skill `SKILL.md` cài được ngay, mẫu để tự viết skill, và liên kết tới các dự án cộng đồng. Mọi đóng góp đều được chào đón — xem hướng dẫn trong [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT — see [LICENSE](LICENSE). Individual skills may note additional attribution where they build on third-party work.
