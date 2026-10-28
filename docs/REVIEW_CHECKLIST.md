# Review checklist

Maintainers (and contributors self-checking) verify every submitted skill against this list before merge.

## Format

- [ ] Folder name matches the frontmatter `name` (lowercase, hyphens).
- [ ] `description` precisely states the task and trigger conditions.
- [ ] Body has ordered steps, guardrails, and a verification step.
- [ ] Long material is in `references/`, scripts in `scripts/` — `SKILL.md` stays focused.

## Safety

- [ ] No secrets, credentials, personal data, or real account identifiers anywhere.
- [ ] No instruction to hide actions from the user, bypass approvals, or ignore user instructions.
- [ ] No instruction to send data to third parties beyond what the task itself requires.
- [ ] Destructive or irreversible actions require explicit user confirmation in the steps.

## Quality

- [ ] Tested on Meta Muse with at least two realistic requests; test evidence in the PR.
- [ ] Steps are executable as written (tool names, file paths, and formats are real).
- [ ] Failure handling is described: what Muse should do when a step fails.
- [ ] Third-party material keeps its license and attribution.

## Listing

- [ ] Row added to the `README.md` skills table.
- [ ] Entry added under the right category in `docs/CATEGORIES.md`.
