# Contributing to Kuma Code

Thanks for thinking about contributing — this project is small enough that every PR matters.

## How to help

- **Code:** fix a bug, ship a feature, refine the agent runtime.
- **Skill packs:** the differentiator only works if the skill packs are good. Building a skill pack for a stack we don't cover yet is high-leverage.
- **Docs:** if a step in this file or the README is unclear, fix it. Doc PRs are reviewed quickly.
- **Bug reports:** open a [GitHub Issue](https://github.com/zerosecai/kuma-code/issues) with a minimal repro.
- **Discussion:** open a [Discussion](https://github.com/zerosecai/kuma-code/discussions) for ideas, design questions, or "is this a bug" triage.

## A note on naming during the rebrand

Kuma Code is a fork of [Kilo Code](https://github.com/Kilo-Org/kilocode). The visual identity and metadata have moved over to Kuma, but the **internal package and folder names** (`kilo-vscode`, `kilo-gateway`, `@kilocode/cli`, etc.) are still the upstream names. That's intentional and tracked — see `BACKLOG.md` for the deferred source-code rebrand. If you see a `kilo-` reference in source, that's the right name for now. Don't rename it in your PR unless your PR is specifically the rebrand task.

## Dev environment

**Requirements:**

- [Bun](https://bun.sh) 1.3.10 or later
- Node.js 20 or later (some upstream native modules call out to it)
- Python 3.10+ (for `node-gyp` to build the one native dep on Windows)
- VS Code (for the extension dev host)

**Setup:**

```bash
git clone git@github.com:zerosecai/kuma-code.git
cd kuma-code
bun install        # 5–10 min first time
```

If `bun install` fails on Windows during the `tree-sitter-powershell` post-install, set the `PYTHON` env var to point at a real Python interpreter (Microsoft Store stubs don't count). See the troubleshooting note in `BACKLOG.md`.

## Building the VS Code extension

```bash
bun run --cwd packages/kilo-vscode compile
```

This runs the full pipeline: CLI binary copy, SDK rebuild, typecheck, lint, esbuild bundle. Output lands in `packages/kilo-vscode/dist/`.

To launch the dev host:

1. Open the repo in VS Code: `code .`
2. Press **F5**.
3. A new VS Code window opens with the extension loaded.

The "Kuma Code" panel should appear in the activity bar.

## Running the CLI

```bash
bun dev               # runs the CLI in TUI mode against the current repo
bun dev <directory>   # runs against a specific directory
```

`bun dev` is the local equivalent of the bundled `kilo` CLI.

## Building a standalone CLI binary

```bash
./packages/opencode/script/build.ts --single
./packages/opencode/dist/@kilocode/cli-<platform>/bin/kilo
```

Replace `<platform>` with `darwin-arm64`, `linux-x64`, `windows-x64`, etc.

## Pull request expectations

- **Issue first.** All PRs should reference an existing issue or discussion. If one doesn't exist, open it — that's where design feedback happens before code is wasted.
- **One logical change per PR.** Don't bundle a refactor with a feature; they get reviewed differently.
- **PR title** uses [conventional commits](https://www.conventionalcommits.org/) prefix: `feat:`, `fix:`, `docs:`, `refactor:`, etc.
- **UI changes** include a screenshot or short clip in the PR body.
- **Logic changes** explain how you tested it. Manual smoke tests are fine for small UI tweaks; logic changes need a test added or one updated.

## Commit message format

See `CONVENTIONS.md` for the full style. The short version:

```
phase<N>: <imperative verb> <what>

<optional body>
```

Subject line in imperative mood, max 72 chars. One logical change per commit.

## Test requirements

```bash
bun run test                          # all tests
bun run test --filter <package>       # one package
bun run typecheck                     # type-check only (fast)
bun run lint                          # eslint
```

Every public function should have at least one test. Bug fixes should include a regression test that would have failed before the fix.

For the VS Code extension specifically:

```bash
bun run --cwd packages/kilo-vscode compile     # full build pipeline
```

This is the same gate the pre-push hook runs. If this passes, your PR will pass CI.

## Pre-push hook

There is a husky pre-push hook that runs `bun typecheck` across the monorepo. On Windows, an upstream symlink quirk (`packages/app/src/custom-elements.d.ts`) currently makes this fail; see `BACKLOG.md` for status. If you need to push around it temporarily, use `--no-verify`, but please call it out in the PR description so the reviewer knows.

## Issue and PR lifecycle

Inactive issues and PRs are auto-closed after a long quiet period. That's not a judgement on quality — older items lose context, and we'd rather restart fresh than try to revive a stale thread. Reopening or re-filing is fine.

## Style preferences

These are conventions we try to follow in **new** code. Forked code from Kilo / Roo / Cline keeps its own style — match the file you're editing.

- **Functions:** keep logic in one function unless splitting buys obvious reuse.
- **Control flow:** prefer early returns over nested `else`.
- **Types:** no `any` without a `// reason: ...` comment on the same line.
- **Variables:** `const` by default.
- **Naming:** short single-word identifiers when descriptive (`cfg`, `err`, `opts`); multi-word when ambiguous.
- **Errors:** typed errors (`throw new SkillNotFoundError(...)`), not bare `Error`.
- **Runtime APIs:** prefer Bun helpers (`Bun.file`, `Bun.write`) over Node's `fs` in new code.

See `CONVENTIONS.md` for the full guide.

## Contact

- General: open a [Discussion](https://github.com/zerosecai/kuma-code/discussions) or email **sam@zerosec-ai.com**.
- Security vulnerabilities: see [SECURITY.md](SECURITY.md).
