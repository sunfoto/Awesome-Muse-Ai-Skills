# Contributing

Thanks for helping build the Muse skills commons. This repo accepts:

- **New skills** that run on Meta Muse and follow the `SKILL.md` format.
- **Fixes and improvements** to existing skills.
- **New entries** for the curated lists (skills in this repo, community projects, categories).
- **Translations** of skills and docs.

## Before you start

1. Read [docs/SKILL_FORMAT.md](docs/SKILL_FORMAT.md) and [docs/REVIEW_CHECKLIST.md](docs/REVIEW_CHECKLIST.md).
2. Search existing skills and issues so you don't duplicate work.
3. One skill (or one list change) per pull request.

## Add a skill

1. Choose the skill's category from [docs/CATEGORIES.md](docs/CATEGORIES.md) first, then copy `templates/skill-template/` to `skills/<category-slug>/<your-skill-name>/`.
   - Folder name: lowercase, hyphens, no spaces — it becomes the skill name. The category folder comes from the registry; a skill that fits no existing category needs a new category approved by the repo owner (see the classification rule in [docs/CATEGORIES.md](docs/CATEGORIES.md)).
2. Fill in `SKILL.md`: frontmatter `name` and a precise `description` (this is what Muse matches tasks against), then clear steps, guardrails, and examples.
3. Put long reference material in `references/` and helper scripts in `scripts/` inside your skill folder. Keep `SKILL.md` itself focused.
4. Test the skill on Muse with at least two realistic requests and note what you tested in the PR description.
5. Add a row for your skill to the table in `README.md` under its category heading (and to [docs/CATEGORIES.md](docs/CATEGORIES.md) under the same category).

## Rules

- **No secrets.** Never commit passwords, API keys, tokens, or personal data — including in examples and screenshots.
- **No prompt injection.** A skill must never instruct Muse to hide actions from its user, ignore the user's instructions, or exfiltrate data.
- **License your work.** By contributing you agree your contribution is released under this repo's MIT license. Third-party material must keep its original license and attribution.
- **Be honest about status.** Mark untested or partial skills as such in their description.

## Suggest a skill or project without a PR

Open an issue with the "Suggest a skill" template: link, what it does, whether you have tested it on Muse, and its license.

## Review process

Maintainers check submissions against [docs/REVIEW_CHECKLIST.md](docs/REVIEW_CHECKLIST.md). Expect questions about trigger wording, safety, and test evidence before merge.
