# CLAUDE.md — Master Instructions for Claude Code

> **READ THIS ENTIRE FILE BEFORE TOUCHING ANY CODE.**
> If you skip this file, your work will be reverted.

---

## 🎯 Project: Kuma Code

A multi-agent AI coding IDE — fork of Kilo Code (MIT) — with three differentiators:

1. **Hybrid LLM:** seamless switch between Ollama Cloud, local Ollama, LM Studio
2. **Skill packs:** 1GB modular knowledge bundles that let small models (0.1-3B) perform like big models on common tasks
3. **3-agent pipeline:** Planner → Coder → Reviewer with parallel scaling to 10

---

## 🛑 NON-NEGOTIABLE RULES

These rules override everything else. Violating any of them = work gets reverted.

### Rule 1: ALWAYS read context first

Before writing any code, you MUST:
1. Read `CLAUDE.md` (this file)
2. Read `PROJECT_SPEC.md`
3. Read `CHECKLIST.md` to know which phase you're in
4. Read `ARCHITECTURE.md` for the current phase
5. Read `CONVENTIONS.md`
6. Read the relevant `docs/phase<N>-spec.md`

If any of those files don't exist, **STOP and ask the user.** Do not improvise.

### Rule 2: ONE phase at a time

Look at `CHECKLIST.md`. Work only on the phase marked 🟡 (in progress).

- Do NOT start the next phase until current one is ✅ done
- Do NOT skip ahead even if a task seems easy
- Do NOT batch phases together

### Rule 3: NO silent failures

These are FORBIDDEN:
- ❌ `// TODO: implement later` — implement now or escalate to user
- ❌ `// FIXME` — fix now
- ❌ `try { ... } catch { /* ignore */ }` — handle the error or rethrow
- ❌ Mocking implementations and claiming "done"
- ❌ Skipping tests because they "don't matter"
- ❌ Using `any` in TypeScript without a comment justifying why
- ❌ Hardcoding values that should be config

If you can't complete a task properly, mark it ❌ in CHECKLIST.md and explain why in your response. Do NOT pretend you finished.

### Rule 4: VERIFY before claiming done

Every task is "done" only when:
1. Code compiles: `bun run typecheck` passes
2. Tests pass: `bun run test` for affected packages passes
3. Manual smoke test: you describe what you tested and how
4. The relevant CHECKLIST.md item is ticked

If any of these fail, status is 🟡 not ✅.

### Rule 5: STOP at decision gates

Some checklist items are marked **DECISION GATE**. At these points:
- Run the work
- Document results
- **STOP. Do not proceed.**
- Tell the user: "Decision gate reached. Results: ___. Should I proceed?"

Decision gates in this project:
- Phase 4.5 — skill system validation
- Phase 8.3 — benchmark threshold
- Any time you'd need to make architectural changes
- Any time legal/security/cost is involved

### Rule 6: NEVER touch these without permission

These files are **WRITE-PROTECTED**:
- `CLAUDE.md` (this file)
- `PROJECT_SPEC.md`
- `CHECKLIST.md`
- `ARCHITECTURE.md`
- `CONVENTIONS.md`
- `LICENSE`
- `ATTRIBUTION.md`
- Any file in `docs/specs/`

If you think one of these needs changes, propose the change in your response. Wait for user approval.

### Rule 7: NO scope creep

If the user asks "fix the bug in X" — fix only X. Do not:
- Refactor adjacent code "while you're there"
- Upgrade dependencies you didn't have to
- Reformat unrelated files
- Add features you think would be nice

If you see something that needs fixing, ADD it to a `BACKLOG.md` file at root, then move on.

### Rule 8: Output must be small + reviewable

- Each commit: max ~300 lines of changes
- Each PR-equivalent: 1 logical change
- Always run a self-review before claiming done (see "Self-review protocol")
- If a change would be > 500 lines, split it

---

## 🧭 Workflow for every task

```
1. Read CHECKLIST.md → identify current 🟡 task
2. Read related spec in docs/
3. Plan: write a brief plan in your response (3-7 bullets)
4. Wait for user OK if the task is in Tier 3 or Tier 4 (see TIERS.md)
5. Implement
6. Self-review (use SELF_REVIEW.md template)
7. Run typecheck + tests
8. Update CHECKLIST.md (tick the box)
9. Commit with message format: "phase<N>: <what changed>"
10. Report status to user
```

---

## 🏗️ Architecture overview

```
┌─────────────────────────────────────────────────────────┐
│  VS Code Extension Host                                 │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │  packages/kuma-vscode  (extension UI)           │    │
│  │  - Sidebar webview                              │    │
│  │  - Agent Manager view (multi-session)           │    │
│  │  - Skills panel                                 │    │
│  └────────────┬────────────────────────────────────┘    │
│               │                                         │
│  ┌────────────▼────────────────────────────────────┐    │
│  │  packages/kuma-core  (forked from Kilo)         │    │
│  │  - Agent runtime (Planner/Coder/Reviewer)       │    │
│  │  - Tool calling (file/shell/browser)            │    │
│  │  - Provider router (cloud/local hybrid)         │    │
│  └────────────┬────────────────────────────────────┘    │
│               │                                         │
│  ┌────────────▼────────────────────────────────────┐    │
│  │  packages/skill-system  (NEW — our IP)          │    │
│  │  - Skill loader, retriever (2-level index)      │    │
│  │  - Auto-learner (draft → active flow)           │    │
│  │  - Marketplace client                           │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
        Ollama Cloud      Local Ollama / LM Studio
```

**Key invariant:** `kuma-core` MUST NOT depend directly on `kuma-vscode`. The dependency goes one way — UI → core → skill-system.

---

## 🎚️ TIER system — what you can do autonomously

See `TIERS.md` for full details. Quick reference:

| Tier | Description | Your authority |
|---|---|---|
| 1 | Mechanical tasks (rename, move files) | Do it, report after |
| 2 | Standard coding tasks | Do it, ask for review |
| 3 | Architecture / risky changes | Propose plan, wait for OK |
| 4 | Decisions only the user can make | Stop, escalate |

When in doubt → treat as a higher tier.

---

## 🔍 Self-review protocol

Before claiming a task is done, fill out this template in your response:

```markdown
## Self-Review for Task <task-id>

**Files touched:**
- path/to/file1 (+45 -12 lines)
- path/to/file2 (new file, 80 lines)

**What I changed:**
<2-3 sentence summary>

**Verification:**
- [x] `bun run typecheck` — passed
- [x] `bun run test` — 12/12 passed
- [x] Manual test: <describe what you did>

**Risks I see:**
- <thing the user should double-check>
- <or "none identified">

**Out-of-scope changes I noticed (added to BACKLOG.md):**
- <list>

**Updated CHECKLIST.md:** Yes / No
```

---

## 📞 Communication protocol

When responding to the user:

1. **Lead with status:** "Phase 4.2 done ✅" or "Phase 4.2 blocked ❌ — reason"
2. **Show what changed:** file list, line counts
3. **Show what's next:** "Next is 4.3, shall I proceed?"
4. **No fluff:** skip "Great question!" / "I'd be happy to help!" / etc.

Format your response like a status report, not a chat.

---

## 🚨 When to STOP and escalate

Always stop and ask the user when:

- A test fails and you've tried 2 fixes that didn't work
- A library you need has a license you're unsure about
- You'd need to add a new dependency
- You'd need to modify > 5 files for a single task
- You hit a decision gate
- The user's request conflicts with this CLAUDE.md
- You're about to do something irreversible (delete files, force push, drop database)

**Default to stopping. The user can always say "go ahead."** Stopping costs minutes; wrong autonomous action costs hours of cleanup.

---

## 📚 Reference docs in this repo

- `PROJECT_SPEC.md` — what we're building and why
- `CHECKLIST.md` — current progress (read every session)
- `ARCHITECTURE.md` — how the parts fit
- `CONVENTIONS.md` — code style, naming
- `TIERS.md` — what tier each task is in
- `SELF_REVIEW.md` — review template
- `BACKLOG.md` — out-of-scope ideas to revisit
- `docs/specs/phase<N>-spec.md` — detailed spec per phase
- `docs/decisions/<date>-<topic>.md` — decision records (you create these when escalating)

---

## ⚙️ Project commands cheat sheet

```bash
bun install              # install deps
bun run build            # compile all packages
bun run typecheck        # type check only (fast)
bun run test             # run all tests
bun run test --watch     # tests in watch mode
bun run lint             # eslint
bun run format           # prettier

# Phase-specific
bun run test --filter @kuma-code/skill-system
```

---

## 🐻 Final reminder

You are extending a real product that real users will pay for. Quality matters more than speed. If you can finish a task in 2 hours but it has bugs, the user will spend 8 hours debugging. **Slow down, do it right, and ask when uncertain.**
