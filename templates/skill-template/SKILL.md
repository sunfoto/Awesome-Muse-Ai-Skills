---
name: skill-template
description: Template for a new Muse skill. Replace this with 1–2 short sentences packed with the keywords users actually say, stating exactly which task this skill covers.
---

# Skill Template (v2)

<!--
HOW TO USE THIS TEMPLATE
- Keep the frontmatter description SHORT (1–2 sentences) but keyword-rich:
  it is the trigger Muse matches user requests against. Long paragraphs dilute matching.
- Every section below is mandatory. If a section genuinely does not apply,
  write "None." and say why — do not delete the section.
-->

## 1. Context

What problem this skill solves, where it sits inside Muse, and which tools or connectors it revolves around.

## 2. Why the hard rules exist

2–4 lines: the original incident or lesson behind this skill's strict rules. This is what stops Muse from improvising around a rule when an odd situation shows up.

## 3. When to use / when NOT to use

- Use when: …
- Do NOT use when: … (and name the skill or route that should take it instead)

## 4. Inputs required

Data, files, accounts, or permissions that must exist before starting — and what to ask the user when something is missing.

## 5. Procedure

Numbered steps. EVERY step states four things:

1. **Action** — what to do.
2. **Tool/command** — the exact tool, command, or surface to use.
3. **Intermediate output** — what artifact or state the step produces.
4. **Error branch** — "if X happens, do Y."

## 6. Output & acceptance thresholds

What the final deliverable looks like, its format — AND the measurable bar it must clear (numbers, counts, durations, verified items). Format alone is not enough for Muse to conclude "done".

## 7. Examples

- **Correct example:** a realistic user request and how this skill handles it.
- **Boundary example:** a similar-sounding request that must route to a DIFFERENT skill — name which one and why.

## 8. Common failures & fixes

The failures this skill actually hits in practice, and the prescribed response to each.

## 9. Verify before reporting done

The checks Muse runs on the result before telling the user it is finished.

## 10. Dependencies & handoff

Connectors or companion skills this skill needs, where its outputs are stored, and who or what receives the result next.

## 11. Guardrails

- Never include secrets, passwords, or API keys in files, messages, or examples.
- Never hide actions from the user or bypass their approvals.
- Confirm with the user before any irreversible or outward-facing action.
