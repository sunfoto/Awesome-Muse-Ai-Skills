# Categories — the registry

This document is the **authoritative category registry** for this repo. Every skill in the repo lives in exactly one domain folder under `skills/`, named by the category slug:

```text
skills/<category-slug>/<skill-name>/SKILL.md
```

## The classification rule (standing rule, set by the repo owner on 2026-10-28)

- Every skill that is developed or updated **must be filed under its correct domain folder** below and **listed in this registry** under that category.
- The reviewer verifies the classification — right folder, right registry entry — **before** the skill is pushed or merged. A mis-filed skill is moved, not merged as-is.
- A skill that fits **none** of the existing categories requires a **new category approved by the repo owner** (a new entry in this registry, with its own slug). Never force-fit a skill into a wrong category just to avoid the approval step.
- Secondary mentions are allowed: a skill may be *mentioned* under another category it genuinely serves, but its folder and its primary registry entry exist in exactly one place.

## Skill domain categories

### 1. Content & media — `content-and-media`

Writing, social posts, images, audio, video scripting and production.

Folder: `skills/content-and-media/`

- [srt-unverified-marker-gate](../skills/content-and-media/srt-unverified-marker-gate/SKILL.md) — pre-delivery gate for SRT subtitle files: full-text scan for unverified-claim markers, per-marker resolution against real sources, zero markers remaining before QA handover or posting.
- [pre-edit-workflow-g0-g7](../skills/content-and-media/pre-edit-workflow-g0-g7/SKILL.md) — gated pre-edit workflow (gates G0–G7) for narrated videos: fact table first, script locked before voice, SRT as the single timeline for music and visuals, numeric QA gate before delivery.

### 2. Coding & development — `coding-and-development`

Code review, testing, refactoring, release workflows, repository automation.

Folder: `skills/coding-and-development/` — _no skills yet; the folder is created when the first skill lands (see the [integration roadmap](INTEGRATION_MAP.md))._

### 3. Data & research — `data-and-research`

Web research, data collection, analysis, reporting.

Folder: `skills/data-and-research/` — _no skills yet; the folder is created when the first skill lands (see the [integration roadmap](INTEGRATION_MAP.md))._

### 4. Productivity — `productivity`

Document drafting, email triage, scheduling, meeting follow-up, spreadsheets, and other office workflows.

Folder: `skills/productivity/` — _no skills yet; the folder is created when the first skill lands (see the [integration roadmap](INTEGRATION_MAP.md))._

## Unclassified format material

Not skills, and deliberately not filed under any domain:

- [skill-template](../templates/skill-template/SKILL.md) — the starter every new skill begins from.
- [example-hello-muse](../skills/example-hello-muse/SKILL.md) — minimal working example of the format; stays at the top level of `skills/` as a demo.

## Curation categories for linked community projects

The topics below group **links to community projects** curated in this repo (see the README). They have no skill folders: if a real skill in one of these areas is ever contributed, it first needs an owner-approved domain category above, per the classification rule.

### Game development

Game design, engine workflows (Godot, Unity, Unreal, web engines), assets, balance, playtesting.

_No entries yet — contributions welcome._

### Business & marketing

SEO, ads, e-commerce operations, customer research, small-business workflows.

_No entries yet — contributions welcome._

### Connectors & integrations

Skills and projects built around a specific service connector (mail, calendar, stores, social platforms).

_No entries yet — contributions welcome._
