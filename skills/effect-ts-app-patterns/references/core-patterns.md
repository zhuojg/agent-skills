# Core App Patterns

These are the preferred application-level patterns for using Effect in normal TypeScript code.

## Mental Model

Treat a program as:

```ts
Effect.Effect<Success, Error, Requirements>
```

- `Success`: the successful result
- `Error`: expected failures
- `Requirements`: services or dependencies the program needs

For most app-level code, the immediate gains come from making `Error` explicit and keeping execution at the edges.

## Prefer `Effect.gen` For Orchestration

For multi-step workflows, use `Effect.gen` first.

```ts
import { Effect } from "effect";

const program = Effect.gen(function* () {
  const session = yield* verifySession(token);
  const profile = yield* loadProfile(session.userId);
  yield* Effect.logInfo("[Profile] Loaded profile");
  return profile;
});
```

Reserve `pipe`-heavy composition for short local transforms where it is clearly shorter and easier to read.

## Wrap Unsafe Boundaries

Use `Effect.try` for sync code that may throw:

```ts
const parseJson = (input: string) =>
  Effect.try({
    try: () => JSON.parse(input) as unknown,
    catch: (cause) => new JsonDecodeError({ cause }),
  });
```

Use `Effect.tryPromise` for async code that may reject:

```ts
const fetchUser = (id: string) =>
  Effect.tryPromise({
    try: () => fetch(`/api/users/${id}`).then((res) => res.json()),
    catch: (cause) => new FetchUserError({ cause }),
  });
```

Keep this wrapping at the boundary where unsafe code enters the program. Do not let thrown exceptions leak through “normal” control flow.

## Use Tagged Errors For Expected Failures

Prefer small, domain-specific tagged errors:

```ts
import { Data } from "effect";

class TokenExpiredError extends Data.TaggedError("TokenExpiredError")<{
  readonly expiresAt: number;
  readonly now: number;
}> {}
```

This keeps recoveries precise and makes error branching explicit.

## Keep Logging In The Program

Log in the pipeline, not beside it:

```ts
const program = Effect.gen(function* () {
  const result = yield* loadAccount(id);
  yield* Effect.logInfo("[Account] Loaded account");
  return result;
});
```

For failures, log in the recovery branch or with `tapError`:

```ts
const withLogging = program.pipe(
  Effect.tapError((error) => Effect.logWarning("[Account] Load failed", error)),
);
```

Use levels intentionally:

- `logInfo` for normal success path events
- `logWarning` for expected failures or degraded paths
- `logError` for operational or unexpected failures

## Recover Explicitly

Use explicit branching instead of hiding the failure path:

```ts
const response = Effect.matchEffect(program, {
  onSuccess: (value) => Effect.succeed({ ok: true as const, value }),
  onFailure: (error) =>
    Effect.gen(function* () {
      yield* Effect.logWarning("[Feature] Request failed", error);
      return { ok: false as const, error: toUserMessage(error) };
    }),
});
```

For tag-specific recovery, prefer `catchTag` / `catchTags` when different domain errors need different fallback behavior.

## Run Only At Boundaries

Keep Effect internal to the feature. Execute it only at framework edges:

- route handlers
- server functions
- event handlers
- command entrypoints

Prefer one shared helper per app boundary style:

```ts
const runAppEffect = <A, E>(program: Effect.Effect<A, E, never>) =>
  Effect.runPromise(program);
```

This keeps execution policy centralized instead of scattering `Effect.runPromise` calls throughout the codebase.

## Scope Limit

This pattern set is intentionally small. If the task needs:

- dependency injection with services
- `Layer` composition
- schema decoding
- streams
- advanced concurrency primitives

go to [`official-docs.md`](./official-docs.md) and read the relevant official pages before deciding on a pattern.

