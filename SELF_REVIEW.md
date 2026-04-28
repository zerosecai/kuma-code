# SELF_REVIEW.md — Template

> Copy this template into your response after completing a Tier 2+ task.
> Filling it out is REQUIRED. Skipping = task not done.

---

## Self-Review for `<phase.X.Y - task name>`

### What I changed

**Files touched:**
- `path/to/file1` — `+45 -12 lines` — <one-line why>
- `path/to/file2` — `new file, 80 lines` — <one-line why>

**Summary:** <2-3 sentences explaining the change in plain language>

---

### Verification

Run each check and paste the actual output (not just "passed"):

**Type check:**
```
$ bun run typecheck
<paste actual output>
```

**Tests:**
```
$ bun run test --filter <package>
<paste actual output>
```

**Lint:**
```
$ bun run lint
<paste actual output>
```

**Manual smoke test:**
<describe step-by-step what you did in VS Code or CLI to verify behavior>

Example:
1. Pressed F5 to launch extension dev host
2. Opened sidebar, typed "make a button"
3. Saw 3 artifacts stream in: planner, coder, reviewer
4. All three rendered correctly
5. No errors in Developer Console

---

### Hand-checks (you, not tools)

- [ ] No `// TODO` left in my changes
- [ ] No `// FIXME` left in my changes
- [ ] No `any` without a `// reason: ...` comment
- [ ] No empty `catch { }` blocks
- [ ] No mocks where real implementations are expected
- [ ] No hardcoded API keys, secrets, or URLs
- [ ] All new public functions have JSDoc
- [ ] All new errors are typed (custom error class, not bare `Error`)
- [ ] Files don't exceed 500 lines
- [ ] No circular imports introduced
- [ ] Followed dependency rules from `ARCHITECTURE.md`

---

### Risks / things to double-check

List anything you're uncertain about. Don't pretend confidence you don't have:

- <risk 1>
- <risk 2>
- (or "None identified")

---

### Out-of-scope items

If you noticed things that need fixing but weren't part of this task:

- Added to `BACKLOG.md`: <list>

(Do NOT fix them in this commit. Scope creep is forbidden — see `CLAUDE.md` Rule 7.)

---

### Tier classification

**This task was Tier:** 1 / 2 / 3 / 4

**If Tier 3:** confirmation that user approved your plan: <quote their message>

**If Tier 4:** you should not have done this autonomously. Escalate.

---

### Status update

- [ ] `CHECKLIST.md` updated — phase `<X.Y>` marked ✅
- [ ] Commit made: `<commit message>`
- [ ] Pushed to branch: `<branch name>`

---

### Next

The next checklist item is: `<X.Y+1 - task name>`

Shall I proceed? **(yes/no)**

---

## Example filled-out review

> Below is what a complete self-review looks like.

## Self-Review for `4.1 - Move skill-system into monorepo`

### What I changed

**Files touched:**
- `packages/skill-system/package.json` — new file, 18 lines — workspace package config
- `packages/skill-system/src/format/types.ts` — new file, 60 lines — copied from prototype
- `packages/skill-system/src/loader/loader.ts` — new file, 56 lines — copied from prototype
- `packages/skill-system/src/retriever/retriever.ts` — new file, 105 lines — copied from prototype
- `packages/skill-system/src/learner/learner.ts` — new file, 89 lines — copied from prototype
- `packages/skill-system/skills/typescript/react/` — copied 5 files
- `packages/skill-system/tests/retrieval.test.ts` — new file, 110 lines — copied from prototype
- `package.json` (root) — `+1 -0` — added workspace entry

**Summary:** Moved the validated skill-system prototype into the Kuma Code monorepo as a workspace package. No code changes — pure file move + workspace registration.

### Verification

**Type check:**
```
$ bun run typecheck
✓ packages/skill-system
✓ packages/kuma-core
✓ packages/kuma-vscode
```

**Tests:**
```
$ bun run test --filter @kuma-code/skill-system
Loaded skill: TypeScript + React
Topics: 3
Running 7 test cases...
✓ topic / ✓ section  | "how do I clean up an interval..."
[... 6 more ...]
Topic accuracy:   7/7 (100.0%)
Section accuracy: 7/7 (100.0%)
✓ MVP threshold met (>= 80%)
```

**Manual smoke test:**
1. `bun install` — workspace linked successfully, saw `packages/skill-system` in output
2. Imported `@kuma-code/skill-system` from `kuma-core` — resolved correctly
3. Ran tests from monorepo root — all pass

### Hand-checks
- [x] All checks ✓ (no new code added, just moved files)

### Risks
- None identified — pure file move, all tests still pass.

### Out-of-scope items
- Noticed `@kuma-code/skill-system` doesn't yet have a README. Added to `BACKLOG.md`.

### Tier
**Tier 1** (mechanical move).

### Status
- [x] `CHECKLIST.md` updated — `4.1` marked ✅ (8/8 sub-items)
- [x] Commit: `phase4: move skill-system into monorepo as workspace package`
- [x] Pushed to `main`

### Next
Next is `4.2 - Wire into agent runtime`. This is **Tier 3** (touches agent core). Shall I propose a plan first?
