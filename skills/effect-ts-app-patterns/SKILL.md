---
name: effect-ts-app-patterns
description: Use when writing or refactoring TypeScript application logic with Effect, especially for typed error handling, side-effect boundaries, logging, explicit recovery, and executing programs at framework edges.
---

# Effect TS App Patterns

## Overview

This skill is intentionally narrow. It does not try to teach all of Effect.

Use it for application-layer Effect code: route handlers, server functions, feature orchestration, async workflows, and business logic that benefits from typed errors and explicit execution boundaries.

It is not a full Effect reference. For broader APIs like `Layer`, `Context`, `Schema`, `Stream`, concurrency primitives, and service architecture, use the official docs linked in [`references/official-docs.md`](./references/official-docs.md).

## When To Use

- Converting Promise-heavy app logic into Effect
- Refactoring thrown errors into typed domain failures
- Writing multi-step server or feature workflows
- Adding logging and recovery inside an Effect program
- Standardizing where `Effect.runPromise`-style execution happens

Do not use this skill as a general “Effect best practices” guide for every module in the library. It is specifically about app patterns.

## Default App Pattern

1. Model the program as `Effect.Effect<Success, Error, Requirements>`.
2. Use `Effect.gen` for multi-step orchestration.
3. Wrap unsafe sync/async boundaries with `Effect.try` / `Effect.tryPromise`.
4. Represent expected failures with `Data.TaggedError`.
5. Keep logs and recoveries inside the Effect pipeline.
6. Execute the program only at framework boundaries.

## What To Read Next

- Read [`references/core-patterns.md`](./references/core-patterns.md) for the preferred coding patterns this skill is built around.
- Read [`references/official-docs.md`](./references/official-docs.md) when the task needs Effect APIs outside this narrow app-pattern scope.

## Common Mistakes

- Throwing raw exceptions for expected domain failures
- Calling `Effect.runPromise` deep in library helpers
- Using `console.log` instead of `Effect.log*` inside Effect-first code
- Hiding all failures behind one generic error type
- Reaching for advanced abstractions before basic effectful boundaries are clean

