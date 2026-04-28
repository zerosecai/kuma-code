# CONVENTIONS.md

> Code style and patterns for Kuma Code. Auto-formatters can't catch
> everything — these are the rules a human reviewer would call out.

---

## TypeScript

### Strictness

- `strict: true` in `tsconfig.json` — never weaken
- `noUncheckedIndexedAccess: true` — array/object access returns `T | undefined`
- `exactOptionalPropertyTypes: true`

### Forbidden

- ❌ `any` without a `// reason: ...` comment on the same line
- ❌ `!` non-null assertion (use a check or default)
- ❌ `// @ts-ignore` (use `// @ts-expect-error` with explanation)
- ❌ Returning `Promise<any>` from public APIs
- ❌ `as` casts unless narrowing a known union
- ❌ Default exports (named only — better tooling)

### Required

- ✅ All public functions have JSDoc with `@param` for non-obvious args
- ✅ All public types are exported from a single `index.ts` per package
- ✅ Errors are typed: `throw new SkillNotFoundError(id)` not `throw new Error(...)`

---

## Naming

- **Variables/functions:** `camelCase`
- **Types/interfaces:** `PascalCase`, no `I` prefix
- **Files:** `kebab-case.ts`
- **Folders:** `kebab-case`
- **Constants:** `SCREAMING_SNAKE` only for module-level constants
- **Booleans:** prefix with `is`, `has`, `should`, `can`
- **Async functions:** verb that implies time (`fetch`, `load`, `compute`) — never `getX` for async

### Single-word names preferred

```ts
// Good
const cfg = loadConfig();
const err = await tryRun();
const opts = { ... };

// Avoid (verbose, low information density)
const configurationOptions = loadConfigurationFile();
const errorResult = await tryToRunOperation();
```

Exception: when single-word is ambiguous, use multi-word. Don't make readers guess.

---

## Error handling

### Forbidden

- ❌ `catch (e) { console.log(e); }` — log AND rethrow OR handle properly
- ❌ Empty catch blocks
- ❌ Swallowing errors with `.catch(() => null)` without comment

### Required

- ✅ Every async function either handles or propagates errors
- ✅ User-facing errors have actionable messages
- ✅ Internal errors include enough context to debug

```ts
// Good
throw new SkillLoadError(
  `Failed to parse manifest at ${manifestPath}: ${err.message}`,
  { cause: err }
);

// Bad
throw new Error('failed');
```

---

## Async / concurrency

- Prefer `async/await` over `.then()`
- Use `Promise.all` for independent parallel work
- Use `Promise.allSettled` when you need all results regardless of failures
- Never use unbounded parallelism — always limit (`p-limit` or manual semaphore)
- Always pass `AbortSignal` for cancellable operations

```ts
// Good
const results = await Promise.all(
  items.slice(0, 10).map(item => process(item, signal))
);

// Bad — can blow up with 1000+ items
const results = await Promise.all(items.map(item => process(item)));
```

---

## File organization

- One concept per file
- File length: aim for < 300 lines, hard limit 500
- If a file grows past 300 lines, split it
- Tests live next to source: `foo.ts` + `foo.test.ts` OR in `__tests__/foo.test.ts`

---

## Imports

- Absolute imports within a package: `import { x } from '@/format/types'`
- Relative imports only for siblings: `import { x } from './helpers'`
- Group order: built-in → external → internal → relative
- Type-only imports use `import type`

---

## Comments

### Good comments

```ts
// We use Bun's file watcher here instead of chokidar because chokidar
// has a known leak on macOS when watching > 100 files.
const watcher = Bun.watch(...);
```

### Bad comments

```ts
// Set x to 1
const x = 1;

// Loop through items
for (const item of items) { ... }
```

Comments explain **why**, not **what**. The code shows what.

---

## Commits

### Format

```
phase<N>: <imperative verb> <what>

<optional body explaining context>

<optional refs: closes #123>
```

### Examples

```
phase4: add SkillRetriever to agent runtime

Wires skill-system into kuma-core/orchestrator. Before each model call,
queries the skill index for matches. If score > 0.6, prepends retrieved
content to the prompt. Falls back to plain prompt otherwise.
```

```
phase2: rebrand Kilo Code to Kuma Code in package.json files
```

### Rules

- Subject: imperative mood ("add" not "added"), max 72 chars
- One logical change per commit
- No "WIP", "fix typo", "save progress" — squash before push
- Reference checklist phase always

---

## Tests

- Unit tests next to source files
- Integration tests in `tests/` per package
- Every public function has at least one test
- Every bug fix has a regression test
- Test names describe behavior: `it('returns null when skill not found')` not `it('test 1')`
- No mocks for things you own — extract an interface and test the real thing

---

## What to do when conventions conflict with upstream Kilo code

You'll see code in `kuma-core/` that violates these conventions because we forked it. Don't reformat it all — that creates merge conflicts when syncing upstream.

Rule:
- **NEW code (yours):** must follow these conventions
- **Modified code:** apply conventions to lines you're already touching
- **Untouched code:** leave it alone

This minimizes upstream sync pain.
