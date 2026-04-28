# TIERS.md

> Authority levels for autonomous Claude Code work. Use the right tier
> for each task. **When in doubt, escalate one tier up.**

---

## Tier 1 — Mechanical

**Authority:** Just do it. Report after.

**Examples:**
- Rename files / variables based on a pattern
- Move files between folders
- Apply codemods (e.g., update import paths)
- Fix lint errors
- Format code
- Add type annotations to inferred types
- Update copyright headers

**Rules:**
- Must be reversible with `git revert`
- Must not change behavior
- Must compile + pass tests after

**Self-check before doing:**
- [ ] If I'm wrong, can the user undo with one command?
- [ ] Am I changing only mechanical things, no logic?

---

## Tier 2 — Standard coding

**Authority:** Plan, do, ask for review.

**Examples:**
- Implement a feature spec'd in `docs/specs/`
- Add a new function
- Fix a bug with a clear repro
- Add tests
- Wire two existing components together
- Write a UI component to a design

**Rules:**
- Must follow `CONVENTIONS.md`
- Must include tests
- Must update relevant docs
- Self-review template required (`SELF_REVIEW.md`)
- One logical change per commit

**Self-check before doing:**
- [ ] Is this in the current 🟡 phase?
- [ ] Do I have a spec or clear requirements?
- [ ] Will my change touch < 5 files?

---

## Tier 3 — Architecture / risky

**Authority:** PROPOSE plan first. Wait for "go ahead." Then execute.

**Examples:**
- Add a new package to the monorepo
- Add a new dependency
- Modify a public API/contract
- Refactor across > 3 files
- Change build config (tsconfig, package.json scripts)
- Modify CI/CD
- Touch security-sensitive code (auth, crypto, secrets)
- Touch concurrency-sensitive code (parallel agents, file locks)
- Anything in `ARCHITECTURE.md`'s "things that look easy but are NOT"

**Required output before coding:**

```markdown
## Plan for <task>

**Goal:** <one sentence>

**Approach:**
1. <step>
2. <step>

**Files I'll touch:**
- path/to/file (why)

**Risks:**
- <thing that could go wrong>

**Alternatives I considered:**
- <alt> — rejected because <reason>

**Tests I'll add:**
- <test>

Awaiting your approval to proceed.
```

**Wait for the user to say "approved" or "go ahead" before writing code.**

---

## Tier 4 — User-only decisions

**Authority:** STOP. Ask the user. Do NOT proceed without explicit answer.

**Examples:**
- Pricing or business model
- Brand / visual decisions
- Trademark / legal questions
- Choosing between two viable architectures
- Renaming the product
- Adding a new top-level feature not in the roadmap
- Deleting a feature
- Anything that would change `PROJECT_SPEC.md`
- Anything that requires the user's accounts (GitHub, Stripe, marketplace)
- Hitting a checklist item marked **DECISION GATE**

**Required output:**

```markdown
## Tier 4 escalation — needs your decision

**Context:** <why this came up>

**The question:** <one clear question>

**Options I see:**
- A: <option> — pros/cons
- B: <option> — pros/cons
- C: <option> — pros/cons

**My recommendation:** <option + brief reason>

I'll wait for your answer before doing anything.
```

**Then STOP. Don't write code until they answer.**

---

## Decision tree

```
Is this on the CHECKLIST.md current phase?
│
├── No → STOP. Wrong phase. Ask user.
│
└── Yes
    │
    └── Does it require user identity / accounts?
        │
        ├── Yes → Tier 4 (escalate)
        │
        └── No
            │
            └── Does it change architecture / API / dependencies?
                │
                ├── Yes → Tier 3 (propose first)
                │
                └── No
                    │
                    └── Is it pure mechanical (no logic change)?
                        │
                        ├── Yes → Tier 1 (just do it)
                        │
                        └── No → Tier 2 (plan light, then do)
```

---

## What "stop" means

Stopping does NOT mean:
- ❌ Writing code anyway and asking after
- ❌ Writing code and marking it "draft"
- ❌ Skipping the task and moving to the next
- ❌ Asking a vague question hoping for permission

Stopping DOES mean:
- ✅ Ending your response with the escalation block
- ✅ Not running any tools that modify files
- ✅ Waiting until the user replies with explicit direction

---

## Cost of mis-tiering

| You did | Should have been | Cost |
|---|---|---|
| Tier 1 instead of 2 | — | Missing tests, lint warnings |
| Tier 2 instead of 3 | — | Wrong design, refactor cost |
| Tier 3 instead of 4 | — | Wrong product direction, days lost |
| Skipped tier (jumped ahead) | — | Trust broken with user |

**Always err on the side of over-escalating.** A 30-second confirmation is cheaper than a 5-hour rework.
