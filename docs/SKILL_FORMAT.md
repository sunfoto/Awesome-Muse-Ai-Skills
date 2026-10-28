# The Muse SKILL.md format

Muse skills follow the open Agent Skills format. A skill is a folder:

```text
<skill-name>/
  SKILL.md            required — the playbook itself
  references/         optional — long reference material Muse reads on demand
  scripts/            optional — helper scripts the skill's steps may call
```

## Frontmatter

`SKILL.md` starts with YAML frontmatter:

```markdown
---
name: my-skill-name
description: One or two sentences stating exactly which task this skill covers and when Muse should use it.
---
```

- `name` must match the folder name: lowercase letters, numbers, and hyphens.
- `description` is the trigger. Muse matches user requests against it, so be specific: name the task, the domain, and any key tools or services. Vague descriptions ("helps with stuff") never fire; over-broad ones fire wrongly.

## Body — the v2 structure

Every skill in this repo follows the same 11 sections, so Muse always knows where to find what:

1. **Context** — the problem, where the skill sits inside Muse, its tools/connectors.
2. **Why the hard rules exist** — 2–4 lines on the incident/lesson behind the strict rules, so Muse never improvises around them.
3. **When to use / when NOT to use** — scope boundaries, naming the route that should take out-of-scope requests.
4. **Inputs required** — data, files, permissions; what to ask when something is missing.
5. **Procedure** — numbered steps; every step states the action, the exact tool/command, the intermediate output, and an error branch ("if X, do Y").
6. **Output & acceptance thresholds** — format AND a measurable bar (counts, durations, verified items).
7. **Examples** — one correct example plus one boundary example that must route to a different skill (named).
8. **Common failures & fixes.**
9. **Verify before reporting done** — the checks run before Muse claims completion.
10. **Dependencies & handoff** — companion connectors/skills, where outputs live, who receives them next.
11. **Guardrails** — no secrets, no hidden actions, confirmation before irreversible steps.

Keep `SKILL.md` lean. Move bulk reference material into `references/` and tell Muse in the body when to read each file.

## Installing a skill in Muse

Copy the skill folder into your Muse custom skills directory (`~/workspace/custom-skills/`) and start a new conversation. Muse indexes skills by name and description.

## Architecture decision — one skill per folder (2026-10-28)

Skills are **never bundled into one mega-skill**. Each skill is its own folder with its own `SKILL.md`, because Muse matches a user request against each skill's short name and description: a bundle dilutes matching, bloats context with unrelated procedures, and makes per-skill review, testing, and selective install impossible. Tightly-coupled steps of one workflow travel as a pack of separate skills (or one skill plus `references/`), never as one giant `SKILL.md`.

## Architecture update — domain folders (2026-10-28)

Skills are also grouped **by domain**. The full path of a skill in this repo is:

```text
skills/<category-slug>/<skill-name>/SKILL.md
```

`<category-slug>` is one of the domain folders defined in the registry in [CATEGORIES.md](CATEGORIES.md): `content-and-media`, `coding-and-development`, `data-and-research`, `productivity`. **Choosing the correct category from that registry is a required step before writing a new skill**: pick the domain first, then create the skill folder inside it. The one-skill-per-folder rule above is unchanged — a domain folder is a grouping level, never a bundle, and a skill that fits no existing category needs a new owner-approved category rather than a force-fit (see the classification rule in [CATEGORIES.md](CATEGORIES.md)). The only exception at the top level of `skills/` is `example-hello-muse`, the unclassified format example.
