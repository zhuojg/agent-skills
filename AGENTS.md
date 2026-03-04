# AGENTS.md

This repository is a multi-skill repo for reusable agent skills.

## Purpose

Use this repo to author and publish multiple installable skills under `skills/`.

This repo intentionally tracks `.agents/` and `skills-lock.json` so the local authoring workflow has a pinned copy of helper skills used to create or update skills.

Do not add other local agent-runtime directories such as `.claude/` unless explicitly requested.

## Where New Skills Go

Create each skill in its own folder:

```text
skills/<skill-name>/
```

Required file:

- `skills/<skill-name>/SKILL.md`

Optional supporting files should only be added when they directly support execution of the skill, such as:

- `agents/openai.yaml`
- `scripts/`
- `references/`
- `assets/`

## Before Creating A New Skill

Always use the `skill-creator` skill before creating a new skill or updating an existing one in this repository.

Use the tracked local install at `.agents/skills/skill-creator/` as the default source for that workflow in this repo.

Check the existing `skills/` directory first.

Avoid duplication:

- do not create a new skill if an existing skill can be extended
- prefer improving an existing skill's `SKILL.md` or supporting files when the scope overlaps
- only create a separate skill when the trigger conditions and workflow are clearly distinct

## Skill Authoring Rules

- Use lowercase hyphenated names for skill folders and the `name` field.
- Keep `SKILL.md` as the installable entrypoint.
- Do not add extra documentation files such as per-skill `README.md` unless explicitly required by a downstream tool.
- Keep only skill-operational files inside that skill's own directory.
- Keep shared repo guidance in root-level files such as `AGENTS.md` and `README.md`, not inside a skill folder.
