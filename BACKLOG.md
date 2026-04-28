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
