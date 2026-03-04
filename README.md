# Agent Skills Publisher Repo

This repository is a publisher-first, multi-skill package for the `npx skills` ecosystem.

It is intended to host multiple reusable skills in one Git repository so they can be installed from other projects with the Skills CLI.

## Repository Layout

- `skills/` contains installable skills.
- `AGENTS.md` defines the local authoring rules for future skill work in this repo.
- `.agents/` contains locally installed helper skills used while authoring new skills in this repository.
- `skills-lock.json` pins the installed helper skills under `.agents/`.

Each installable skill should live in its own directory:

```text
skills/<skill-name>/
  SKILL.md
  ...
```

`SKILL.md` is the required installable entrypoint. Additional files should only be included when they directly support the skill's operation, such as bundled scripts, references, assets, or agent metadata.

## Installing From This Repo

Examples with the Skills CLI:

```bash
npx skills add <owner>/<repo> --list
npx skills add <owner>/<repo> --skill <skill-name>
npx skills add <owner>/<repo> --skill '*'
```

You can also install from a full Git URL once the repository is published.

## Adding The First Skill

1. Create `skills/<skill-name>/`.
2. Add a valid `SKILL.md` with YAML frontmatter.
3. Use the `skill-creator` skill to shape the skill contents and supporting files.
4. Check existing skills first to avoid overlap.
5. Commit the new skill directory.

See `AGENTS.md` for the repo conventions.
