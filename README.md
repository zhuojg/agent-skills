# Agent Skills Publisher Repo

This repository is a publisher-first, multi-skill package for the `npx skills` ecosystem.

It is a curated personal skill collection: reusable agent skills extracted from real project work, then generalized so they can be installed and reused across other repositories.

The goal is to keep each skill narrow, practical, and implementation-focused:

- capture repeatable coding or integration patterns
- avoid duplicating large third-party docs
- link to official documentation when a topic is broader than the skill's scope

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

## Included Skills

### `paraglide-js`

A general Paraglide JS integration skill for i18n work.

It focuses on the practical patterns needed to use Paraglide in real codebases:

- Vite-based setup and generated output usage
- locale strategy ordering and custom strategies
- SSR integration with `paraglideMiddleware()`
- TanStack Start integration
- monorepo setup, especially the "each package compiles" pattern

It is general-purpose, but it also captures a proven monorepo pattern based on real TanStack Start usage.

### `effect-ts-app-patterns`

A narrow Effect skill for application-layer TypeScript code.

It does not try to document the full Effect library. Instead, it focuses on the app patterns that are repeatedly useful in day-to-day feature work:

- `Effect.Effect<A, E, R>` as the basic program model
- `Effect.gen` for multi-step workflows
- `Effect.try` / `Effect.tryPromise` for unsafe boundaries
- `Data.TaggedError` for expected failures
- in-pipeline logging and explicit recovery
- executing programs only at framework boundaries

For advanced topics outside that scope, the skill points directly to the official Effect documentation.

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
