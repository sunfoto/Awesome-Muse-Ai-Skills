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

## Body

Write the body for an agent, not a human reader:

1. **When to use / when not to use** — scope boundaries.
2. **Steps** — an ordered, concrete procedure. Prefer checkable actions over advice.
3. **Guardrails** — what never to do (secrets, destructive actions, unapproved sends/purchases).
4. **Quality checks** — how Muse verifies the result before reporting done.
5. **Examples** — one or two short example requests and the expected shape of the result.

Keep `SKILL.md` lean. Move bulk reference material into `references/` and tell Muse in the body when to read each file.

## Installing a skill in Muse

Copy the skill folder into your Muse custom skills directory (`~/workspace/custom-skills/`) and start a new conversation. Muse indexes skills by name and description.
