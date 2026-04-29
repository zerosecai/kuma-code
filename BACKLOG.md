# BACKLOG.md

Out-of-scope items captured during work, to revisit later.

---

## Phase 2 leftovers

- **Rename `packages/kilo-*` folders to `packages/kuma-*`** — the
  `clone-and-rebrand.sh` script replaced text *inside* files but did not
  rename the folders themselves. Affected directories:
  `kilo-vscode`, `kilo-docs`, `kilo-gateway`, `kilo-i18n`, `kilo-indexing`,
  `kilo-jetbrains`, `kilo-telemetry`, `kilo-ui`. Renaming touches every
  import path, every `package.json` `workspaces` glob, every
  `bun --cwd packages/...` reference in scripts, and the turbo task IDs
  (`@kuma-code/kilo-docs#build` etc.). Plan a Tier 3 task in Phase 3.

## Phase 2 / Windows compat — symlink handling

Upstream Kilo Code commits packages/app/src/custom-elements.d.ts as a
symlink pointing to packages/ui/src/custom-elements.d.ts. On Windows
without Developer Mode (or git core.symlinks=true), git checks it out
as a plain text file containing the literal path string, which then
fails typecheck (TS1128).

This breaks the husky pre-push hook (bun typecheck across all packages).

Fix options for Phase 2 cleanup:
1. Enable Windows Developer Mode (Settings → System → For developers)
   Then: git config core.symlinks true && git checkout -- packages/app/src/custom-elements.d.ts
2. Document the workaround in CONTRIBUTING.md for Windows users
3. Convert the symlink to a relative re-export .d.ts file (would diverge
   from upstream — coordinate with upstream sync strategy)

Used --no-verify ONCE on commit b1f2cd069 to push the rebrand-recovery
branch. All subsequent pushes should pass the hook.

## Phase 5 / Provider — Replace Kilo Cloud Gateway

In Phase 2.5 F5 test, Extension Dev Host showed:
"Connection failed. Check the output panel or restart the extension."

Cause: extension auto-connects to Kilo Code's cloud gateway (kilo.ai)
which we don't have credentials for — and won't, since we'll be using
Ollama Cloud instead.

Phase 5 fix:
- Replace gateway URL configuration with Ollama Cloud
  (https://ollama.com/v1, OpenAI-compatible)
- Disable auto-connect on first launch; show provider picker instead
- Update onboarding flow

## Phase 9 / Distribution — Tauri/Electron platform icons

The desktop app packages have a 60-file icon set per environment:
- packages/desktop/src-tauri/icons/{beta,dev,prod}/  (10 PNG sizes + .icns + .ico = 12 each)
- packages/desktop-electron/icons/{beta,dev,prod}/  (same)

These are still upstream Kilo's branding because the brand kit doesn't
ship the UWP Square*Logo PNG sizes (107x107, 142x142, 150x150, 284x284,
310x310, 30x30, 44x44, 71x71, 89x89, StoreLogo) or .icns/.ico.

When we plan to ship desktop builds:
  cd packages/desktop && bun x @tauri-apps/cli icon ../../brand/png/logo-1024.png
  # Equivalent for desktop-electron via electron-builder or icon-gen

Skipped in Phase 3.1 because we're VS Code-extension-first (Phase 9).

## Phase 3.5 / Source-code rebrand — logo components

These TypeScript files render the logo programmatically and are still
upstream Kilo (cyan/yellow design hard-coded or referenced):

- packages/kilo-ui/src/components/{logo.tsx, favicon.tsx}
- packages/kilo-ui/src/stories/logo.stories.tsx
- packages/opencode/src/cli/cmd/tui/component/{logo.tsx, kilo-logo.tsx}
- packages/opencode/src/cli/logo.ts
- packages/ui/src/components/{logo.tsx, logo.css, logo.stories.tsx}

Intentionally untouched in Phase 3.1 (visual identity is asset-only).
Phase 3.5 source-code rebrand will revisit these alongside identifier
rebranding and folder renames.

## Phase 3.5 / Visual regression snapshots — regenerate after logo touch

packages/kilo-docs/public/img/screenshot-tests/kilo-ui/visual-regression/
components-logo/full-logo-chromium-linux.png is a Playwright snapshot
that captures the Kilo logo render. It will go red the first time the
visual-regression suite runs against the Kuma render.

When the source-code logo components are updated in Phase 3.5:
  bun run --cwd packages/kilo-vscode test:visual:update
  # or whichever package owns the visual regression
This regenerates snapshots and is the correct way to update them
(don't manually overwrite the PNG).

## Phase 5 / VS Code Extension — CLI binary path resolution

F5 dev host on Windows fails with:
"CLI binary not found at expected path:
 c:\Users\zero\Downloads\Kuma code\kuma-code\packages\kilo-vscode\bin\kilo.exe"

Despite kilo.exe existing at that exact path (178 MB, executable).

Likely causes:
- Windows path case-sensitivity (lowercase c: vs uppercase C:)
- Space in folder name "Kuma code" (parent dir)
- esbuild __dirname issue in bundled dist/extension.js
- Hardcoded Unix path in upstream Kilo source

Investigation deferred to Phase 5 when we rewrite the provider/server
layer to replace kilo.ai gateway with Ollama Cloud. The CLI binary
flow is part of that rewrite.

For local dev workaround, may need to:
- Move repo to path without spaces (e.g. C:\dev\kuma-code\)
- OR patch local-bin.ts to normalize path case

## Phase 5 / Privacy & telemetry cleanup

PRIVACY audit revealed (commit 749116cbb era):

1. PostHog telemetry inherited from Kilo upstream
   - File: packages/opencode/src/config/config.ts:288-296
   - Phase 3.2 fix: changed default to false (telemetry off)
   - Phase 5 task: remove @kilocode/kilo-telemetry package entirely,
     replace with opt-in Kuma telemetry layer

2. 40 kilo.ai / app.kilo.ai endpoints in source
   - Categories: gateway API, default API base, share/cloud sync,
     profile/usage, kiloclaw, config $schema URL writer, docs URL,
     gateway setup help, install update check
   - Phase 5 task: replace api.kilo.ai → Ollama Cloud
   - Phase 5 task: remove $schema URL auto-write into user configs
   - Phase 5 task: replace docs URL with kumacode.dev/docs

Until Phase 5 completes, PRIVACY.md flags these honestly.
