# 🐻 Kuma Code — End-to-End Checklist

**Last updated:** 2026-04-29
**Status legend:** ✅ done · 🟡 in progress · ⬜ todo · ⏭️ skipped · ❌ blocked

> Update status by changing `⬜` to `🟡` when starting, `✅` when done.

> **Source of truth:** this file in the repo (`kuma-code/CHECKLIST.md`) is now the live tracker. The copy in `kuma-code-setup/` was the original spec from Phase 0; treat this in-repo copy as authoritative for status from 2026-04-29 onward.

---

## 🎯 Phase 0 — Decisions ✅

- [x] ✅ Product name: **Kuma Code**
- [x] ✅ Tagline: "Multi-agent coding IDE with skill packs"
- [x] ✅ Base repo: `Kilo-Org/kilocode` (MIT, 17K stars)
- [x] ✅ License compatibility verified (MIT allows commercial fork)
- [x] ✅ Architecture: 3-agent MVP → 10-agent scale
- [x] ✅ Backend: Hybrid (Ollama Cloud + local Ollama + LM Studio)
- [x] ✅ Differentiator: 1GB skill packs + auto-learn from API calls
- [x] ✅ MVP test domain: TypeScript + React + Vite + Vitest
- [x] ✅ Hardware target: 32GB RAM, 4TB SSD (corporate spec)
- [x] ✅ Skill scope: hybrid hierarchy (language/framework/topic)
- [x] ✅ Auto-learn flow: draft → active (👍) / discarded (👎)

---

## 🧪 Phase 1 — Skill System Prototype ✅

Built and tested. Prototype lives in `skill-system/` zip from earlier.

- [x] ✅ Skill format spec (`docs/SKILL_FORMAT.md`)
- [x] ✅ TypeScript types (`src/format/types.ts`)
- [x] ✅ Skill loader (`src/loader/loader.ts`)
- [x] ✅ 2-level retriever (`src/retriever/retriever.ts`)
- [x] ✅ Auto-learner (`src/learner/learner.ts`)
- [x] ✅ Sample skill `typescript/react` (3 topics, 8 sections)
- [x] ✅ Test suite (7 cases)
- [x] ✅ **Test passed: 7/7 topic, 7/7 section (100%)**
- [x] ✅ TypeScript compiles cleanly

---

## 🔧 Phase 2 — Fork & Rebrand ✅

Run on YOUR machine.

### 2.1 Pre-flight checks

- [ ] ⬜ Bun installed (`bun --version`)
- [ ] ⬜ Node 20+ installed (`node --version`)
- [ ] ⬜ Git installed (`git --version`)
- [ ] ⬜ VS Code installed
- [ ] ⬜ GitHub account ready
- [ ] ⬜ SSH key works (`ssh -T git@github.com`)
- [ ] ⬜ 5GB+ free disk space
- [ ] ⬜ Stable internet

### 2.2 Fork on GitHub UI

- [ ] ⬜ Visit https://github.com/Kilo-Org/kilocode
- [ ] ⬜ Click "Fork"
- [ ] ⬜ Repo name: `kuma-code`
- [ ] ⬜ Description: "Multi-agent AI coding IDE with skill packs"
- [ ] ⬜ Keep "Copy main branch only" checked
- [ ] ⬜ Click "Create fork"
- [ ] ⬜ Verify fork at `github.com/YOUR_USERNAME/kuma-code`

### 2.3 Run automated rebrand

- [ ] ⬜ Download `kuma-code-setup.zip`
- [ ] ⬜ Unzip in working folder
- [ ] ⬜ `cd kuma-code-setup`
- [ ] ⬜ `chmod +x clone-and-rebrand.sh`
- [ ] ⬜ `./clone-and-rebrand.sh YOUR_GITHUB_USERNAME`
- [ ] ⬜ Script: 6/6 steps complete without errors
- [ ] ⬜ `kuma-code/` folder exists
- [ ] ⬜ Branch `kuma-rebrand` has first commit

### 2.4 Verify build

- [ ] ⬜ `cd kuma-code`
- [ ] ⬜ `bun install` (5-10 min)
- [ ] ⬜ `bun run build` (3-5 min)
- [ ] ⬜ Build succeeds, `out/` or `dist/` exists
- [ ] ⬜ `bun run typecheck` passes (if available)

### 2.5 Verify in VS Code

- [ ] ⬜ `code .`
- [ ] ⬜ Press F5 → Extension Dev Host opens
- [ ] ⬜ New window shows Kuma Code icon
- [ ] ⬜ Sidebar opens, title "Kuma Code"
- [ ] ⬜ No errors in Developer Console
- [ ] ⬜ Take screenshot

### 2.6 Push and merge

- [ ] ⬜ `git checkout main`
- [ ] ⬜ `git merge kuma-rebrand`
- [ ] ⬜ `git push origin main`
- [ ] ⬜ Verify commit on GitHub
- [ ] ⬜ README on GitHub shows Kuma Code

---

## 🎨 Phase 3 — Manual Rebrand 🟡

### 3.1 Visual identity

- [ ] ⬜ Design logo (bear + code, 512×512 PNG)
- [ ] ⬜ Replace `logo.png` at repo root
- [ ] ⬜ Create monochrome SVG for activity bar
- [ ] ⬜ Replace icon refs in extension manifest
- [ ] ⬜ Re-take README screenshots
- [ ] ⬜ Pick brand colors

### 3.2 Documentation

- [ ] ⬜ Rewrite top of `README.md`
- [ ] ⬜ Create `ATTRIBUTION.md` (Kilo, Roo, Cline)
- [ ] ⬜ Add Kilo MIT license to ATTRIBUTION
- [ ] ⬜ Update `LICENSE` with your copyright
- [ ] ⬜ Update `PRIVACY.md`
- [ ] ⬜ Update `SECURITY.md`
- [ ] ⬜ Update `CODE_OF_CONDUCT.md` contact

### 3.3 Online presence

- [ ] ⬜ Register domain (kumacode.dev)
- [ ] ⬜ Set up DNS
- [ ] ⬜ Twitter/X `@kumacode`
- [ ] ⬜ Discord server
- [ ] ⬜ Email: hello@kumacode.dev
- [ ] ⬜ GitHub repo description
- [ ] ⬜ GitHub repo topics
- [ ] ⬜ GitHub repo homepage URL

### 3.4 Marketplace publisher accounts

- [ ] ⬜ Azure DevOps account (free)
- [ ] ⬜ VS Code Marketplace publisher `kuma-code`
- [ ] ⬜ Verify publisher ID matches package.json
- [ ] ⬜ Generate Personal Access Token
- [ ] ⬜ OpenVSX publisher account

### 3.5 Remove upstream endpoints

- [ ] ⬜ `grep -r "kilo.ai" --include="*.ts" .`
- [ ] ⬜ `grep -r "kilocode.com" --include="*.ts" .`
- [ ] ⬜ Decide: disable cloud OR build own gateway
- [ ] ⬜ Replace/stub each hardcoded URL
- [ ] ⬜ Remove telemetry endpoints
- [ ] ⬜ Verify no calls home

### 3.6 Trademark check

- [ ] ⬜ USPTO search "Kuma Code"
- [ ] ⬜ WIPO search
- [ ] ⬜ Thai DIP search (if targeting Thailand)
- [ ] ⬜ Decide: file or wait
- [ ] ⬜ Budget $250-1000 USD if filing

---

## 🧠 Phase 4 — Integrate Skill System 🟡

### 4.1 Move into monorepo

- [ ] ⬜ Copy `skill-system/src/*` → `packages/skill-system/src/`
- [ ] ⬜ Copy `skill-system/docs/*` → `packages/skill-system/docs/`
- [ ] ⬜ Copy `skill-system/skills/*` → `packages/skill-system/skills/`
- [ ] ⬜ Copy `skill-system/tests/*` → `packages/skill-system/tests/`
- [ ] ⬜ Create `packages/skill-system/package.json`
- [ ] ⬜ Add to root workspaces array
- [ ] ⬜ `bun install` to link
- [ ] ⬜ Run skill-system tests: 7/7 still pass

### 4.2 Wire into agent runtime

- [ ] ⬜ Identify Kilo's agent runtime entry point
- [ ] ⬜ Init `SkillRetriever` on agent start
- [ ] ⬜ Hook: query skill index before model call
- [ ] ⬜ High-confidence match → prepend context
- [ ] ⬜ Config: `kumaCode.skills.enabled` (default true)
- [ ] ⬜ Config: `kumaCode.skills.minScore`
- [ ] ⬜ Test: confirm skill content gets prepended

### 4.3 UI: Installed Skills panel

- [ ] ⬜ New view "Skills" in activity bar
- [ ] ⬜ List installed skills (name, version, size)
- [ ] ⬜ Show last-used per skill
- [ ] ⬜ "Browse marketplace" button (placeholder)
- [ ] ⬜ "Import skill from folder" for dev
- [ ] ⬜ "Disable skill" toggle

### 4.4 UI: Save as skill (auto-learn)

- [ ] ⬜ Command: "Save current task as skill"
- [ ] ⬜ Post-task prompt 👍 / 👎 / Skip
- [ ] ⬜ 👍 → save active example
- [ ] ⬜ 👎 → save discarded
- [ ] ⬜ Skip → save draft (auto-promote 7d)
- [ ] ⬜ Notification "Saved to typescript/react"

### 4.5 Validation tests

- [ ] ⬜ Run 30 React tasks, skills enabled
- [ ] ⬜ Run same 30 tasks, skills disabled (control)
- [ ] ⬜ Compare tokens, latency, quality
- [ ] ⬜ Document in `docs/phase4-results.md`
- [ ] ⬜ **Decision gate:** if ≥ 70% of cloud-only quality → proceed

---

## 🔌 Phase 5 — Provider Layer ⬜

### 5.1 Default provider config

- [ ] ⬜ Default first run: Ollama Cloud
- [ ] ⬜ Onboarding: link to ollama.com/settings
- [ ] ⬜ Preset list (Cloud / Local / Hybrid)
- [ ] ⬜ Document each preset

### 5.2 Local Ollama auto-detect

- [ ] ⬜ Probe `localhost:11434/api/version`
- [ ] ⬜ List local models in dropdown
- [ ] ⬜ Status: "Local Ollama detected (3 models)"
- [ ] ⬜ Per-model config (context, timeout)

### 5.3 LM Studio auto-detect

- [ ] ⬜ Probe `localhost:1234/v1/models`
- [ ] ⬜ List LM Studio models
- [ ] ⬜ Status: "LM Studio detected"

### 5.4 Hybrid routing

- [ ] ⬜ `ProviderRouter` class
- [ ] ⬜ Rule: skill match → small local
- [ ] ⬜ Rule: complexity high → cloud
- [ ] ⬜ Rule: file private → force local
- [ ] ⬜ Rule: budget exceeded → force local
- [ ] ⬜ Show pick + reason in UI
- [ ] ⬜ Per-task override: "Always cloud"

### 5.5 Cost tracking

- [ ] ⬜ Track tokens per request
- [ ] ⬜ Calculate cost per provider
- [ ] ⬜ Status bar running total
- [ ] ⬜ Monthly budget cap setting
- [ ] ⬜ Warn 80%, hard stop 100%

---

## 🤖 Phase 6 — 3-Agent Pipeline ⬜

### 6.1 Customize modes

- [ ] ⬜ "Architect" → "Planner" in UI
- [ ] ⬜ "Code" → "Coder" in UI
- [ ] ⬜ "Debug" → "Reviewer" in UI
- [ ] ⬜ Update mode prompts
- [ ] ⬜ Mode-specific skill scopes

### 6.2 Orchestrator

- [ ] ⬜ Command "Run full pipeline"
- [ ] ⬜ Plan → Code → Review automatic
- [ ] ⬜ Save Artifact each stage
- [ ] ⬜ Persist to `.kuma/runs/<run-id>.json`
- [ ] ⬜ UI: collapsible artifact cards
- [ ] ⬜ "Re-run from stage X"

### 6.3 Parallel agents (3 → 10)

- [ ] ⬜ Planner emits N subtasks (default 3)
- [ ] ⬜ Spawn N Coders via Promise.all
- [ ] ⬜ Use Kilo's git worktree isolation
- [ ] ⬜ Reviewer aggregates
- [ ] ⬜ Re-route fails (max 2 retries)
- [ ] ⬜ Test 5 parallel
- [ ] ⬜ Test 10 parallel
- [ ] ⬜ Verify rate limits handled

### 6.4 Agent Manager view

- [ ] ⬜ Verify Kilo's Agent Manager renders
- [ ] ⬜ Custom column headers/icons
- [ ] ⬜ Per-agent provider indicator
- [ ] ⬜ Per-agent skill indicator
- [ ] ⬜ Real-time cost ticker

---

## 📦 Phase 7 — Skill Marketplace ⬜

### 7.1 Skill pack format

- [ ] ⬜ Decide tar.gz vs zip
- [ ] ⬜ SHA-256 checksum in manifest
- [ ] ⬜ Optional signature (paid skills)
- [ ] ⬜ Test pack/unpack 1GB end-to-end
- [ ] ⬜ Benchmark unpack time

### 7.2 Hosting

- [ ] ⬜ Choose CDN (R2 / S3 / GitHub Releases)
- [ ] ⬜ Set up bucket
- [ ] ⬜ URL pattern: `skills.kumacode.dev/<id>/<version>.skill.tar.gz`
- [ ] ⬜ Signed URLs for paid skills
- [ ] ⬜ Cache headers (immutable per version)

### 7.3 Skill registry

- [ ] ⬜ Create `skills.json` registry
- [ ] ⬜ Host at `skills.kumacode.dev/registry.json`
- [ ] ⬜ Metadata: id, version, size, desc, tags, downloads
- [ ] ⬜ Auto-update on publish

### 7.4 In-app browse + install

- [ ] ⬜ Browser UI (search + tag filter)
- [ ] ⬜ Show: desc, size, downloads, updated
- [ ] ⬜ "Install" downloads + extracts + reloads
- [ ] ⬜ Download progress
- [ ] ⬜ Verify checksum
- [ ] ⬜ "Update available" badge

### 7.5 Build first 5 skills

- [ ] ⬜ `typescript/react` 1GB
- [ ] ⬜ `python/django` 1GB
- [ ] ⬜ `typescript/vue` 1GB
- [ ] ⬜ `python/fastapi` 1GB
- [ ] ⬜ `golang/std` 1GB
- [ ] ⬜ Each: 100+ topics, 500+ sections
- [ ] ⬜ Test small model accuracy ≥ 70% per skill

---

## 🧪 Phase 8 — Quality Validation ⬜

### 8.1 Benchmark suite

- [ ] ⬜ Define 100-task benchmark (50 React, 30 Python, 20 mixed)
- [ ] ⬜ Each: input, expected, criteria
- [ ] ⬜ Save as JSON

### 8.2 Run benchmarks

- [ ] ⬜ Baseline: Kilo + Claude Sonnet (cloud only)
- [ ] ⬜ Baseline: Kilo + qwen2.5-coder:1.5b (local only)
- [ ] ⬜ Kuma: hybrid with skills
- [ ] ⬜ Measure: accuracy, latency, cost per task

### 8.3 Document

- [ ] ⬜ `BENCHMARK_RESULTS.md`
- [ ] ⬜ Comparison table
- [ ] ⬜ Cost-per-task chart
- [ ] ⬜ **Decision gate:** if ≥ 80% accuracy at 30% cost → launch

---

## 🚀 Phase 9 — Distribution ⬜

### 9.1 Build artifacts

- [ ] ⬜ `.vsix` extension package
- [ ] ⬜ Test install from .vsix on clean VS Code
- [ ] ⬜ CLI binaries (Mac arm64/x64, Linux x64, Windows x64)
- [ ] ⬜ Test each on target OS

### 9.2 VS Code Marketplace

- [ ] ⬜ `vsce package`
- [ ] ⬜ `vsce publish` with PAT
- [ ] ⬜ Verify listing
- [ ] ⬜ Add screenshots, gif demo
- [ ] ⬜ Detailed description

### 9.3 OpenVSX

- [ ] ⬜ `npx ovsx publish`
- [ ] ⬜ Verify on open-vsx.org
- [ ] ⬜ Test from VSCodium / Cursor

### 9.4 Landing page

- [ ] ⬜ Build (Astro / Next.js)
- [ ] ⬜ Hero: tagline + screenshot + install
- [ ] ⬜ Features section
- [ ] ⬜ Pricing tiers
- [ ] ⬜ FAQ
- [ ] ⬜ Footer (GitHub, Discord, docs)
- [ ] ⬜ Deploy (Vercel / Cloudflare Pages)

### 9.5 Documentation site

- [ ] ⬜ Choose Docusaurus / Mintlify / VitePress
- [ ] ⬜ Write Getting Started, Config, Skills, API
- [ ] ⬜ Deploy to docs.kumacode.dev
- [ ] ⬜ Add search

### 9.6 Demo content

- [ ] ⬜ Record 60s demo video
- [ ] ⬜ Upload YouTube
- [ ] ⬜ Embed on landing
- [ ] ⬜ 3 blog posts (launch, hybrid LLM, skills)

---

## 💰 Phase 10 — Pricing & GTM ⬜

### 10.1 Pricing model

- [ ] ⬜ Free: extension + 3 skills + local-only
- [ ] ⬜ Pro $10-20/mo: all skills + cloud sync + auto-learn
- [ ] ⬜ Team $30-50/seat/mo: shared skills + admin
- [ ] ⬜ Enterprise: custom on-prem + SSO + audit

### 10.2 Payment infrastructure

- [ ] ⬜ Stripe / Paddle / LemonSqueezy account
- [ ] ⬜ Subscription management
- [ ] ⬜ License key generation
- [ ] ⬜ License validation in extension
- [ ] ⬜ Thai tax invoicing (if applicable)

### 10.3 Beta program

- [ ] ⬜ Recruit 10 beta users
- [ ] ⬜ Beta Discord channel
- [ ] ⬜ Weekly feedback collection
- [ ] ⬜ Iterate 2-4 weeks

### 10.4 Public launch

- [ ] ⬜ Product Hunt
- [ ] ⬜ Hacker News
- [ ] ⬜ Reddit (r/programming, r/LocalLLaMA)
- [ ] ⬜ Twitter thread + demo video
- [ ] ⬜ Email beta users for reviews
- [ ] ⬜ Monitor 48 hrs

### 10.5 Post-launch

- [ ] ⬜ Analytics (Plausible / PostHog)
- [ ] ⬜ Error tracking (Sentry)
- [ ] ⬜ Weekly usage report
- [ ] ⬜ First paying customer 🎉
- [ ] ⬜ Plan v2 roadmap from data

---

## 📊 Progress Summary

```
Phase  0: Decisions             ✅  11/11   (100%)
Phase  1: Skill prototype       ✅   9/9    (100%)
Phase  2: Fork & rebrand        ✅  30/30   (100%)
Phase  3: Manual rebrand        🟡  25/30   (83%)
   3.1 Visual identity          ✅   6/6
   3.2 Documentation            ✅   7/7
   3.3 Online presence          ✅   8/8
   3.4 Marketplace accounts     ⬜   0/5
   3.5 Remove upstream endpoints⬜   0/6
   3.6 Trademark check          ✅   4/5    (initial done; USPTO filing deferred until traction)
Phase  4: Integrate skills      🟡   3/22   (14%)
   pack repo created (kuma-pack-tsreact), M1 build pipeline ✅, M2 content scaling 🟡 (164 chunks)
Phase  5: Provider layer        ⬜   0/19   (0%)
Phase  6: 3-agent pipeline      ⬜   0/19   (0%)
Phase  7: Skill marketplace     ⬜   0/24   (0%)
Phase  8: Quality validation    ⬜   0/9    (0%)
Phase  9: Distribution          ⬜   0/24   (0%)
Phase 10: Pricing & GTM         ⬜   0/22   (0%)
─────────────────────────────────────────────────
TOTAL                              78/219   (36%)
```

**Current focus:** Phase 4 (skill pack content scaling) and Phase 3.4–3.5 (marketplace accounts + endpoint cleanup) running in parallel.

---

## 📝 Decision & Issue Log

| Date | Phase | Decision / Issue | Notes |
|---|---|---|---|
| 2026-04-28 | 0 | Use Kilo Code as base | 17K stars, MIT, has Agent Manager |
| 2026-04-28 | 0 | Build skill-system separately first | Validated thesis pre-integration |
| 2026-04-28 | 0 | Name: Kuma Code | Avoids Kuma mesh confusion |
| 2026-04-28 | 1 | 7/7 retrieval tests passing | 100% with keyword scoring |
| 2026-04-29 | 3.3 | Phase 3.3 closed | Landing page live at zerosec-ai.com (HTTPS, Let's Encrypt). Email kuma@zerosec-ai.com active at Hostinger. README has badges + cross-link to kuma-pack-tsreact. GitHub repo metadata complete. |
| 2026-04-29 | 3.6 | Trademark initial check done | "Kuma Code" name SAFE TO USE. USPTO search clean; Kong's "Kuma" service mesh = different IC class. USPTO filing deferred until traction (1000+ users). Defensive social handles tracked in BACKLOG. |
| 2026-04-29 | 3.2.5 | Telemetry default flipped to false | Privacy disclosure honest. SDK doc strings synced via openapi.json source-of-truth. Removes inherited PostHog opt-out-only behavior from upstream Kilo. |
| 2026-04-29 | 4 D1 | kuma-pack-tsreact repo created | Embedding test 5/5 correct top match, 384-dim vectors, bge-small-en-v1.5 (Xenova), avg 32 ms/chunk on local CPU. |
| 2026-04-29 | 4 M1 | Build pipeline shipped | chunk → embed → index → package → .kpack archive. Pipeline ~7s on 9 sample chunks, 52% compression. Scripts: chunk.ts/embed.ts/index.ts/package.ts. |
| 2026-04-29 | 4 M2 | Content scaling progress | 164 chunks from real TS handbook (Apache 2.0) + react.dev (MIT) + vitejs/vite docs (MIT). 766 KB packed. ATTRIBUTION.md added. Day 4+ targets ~1 GB. |

> Add new rows as you make decisions or hit blockers.

---

## ⏱️ Estimated timeline

| Phase | Time | Notes |
|---|---|---|
| 2 | 2-4 hours | Fork, rebrand script, build, verify |
| 3 | 1-2 weeks | Logo, accounts, manual rebranding |
| 4 | 1-2 weeks | Skill integration + UI |
| 5 | 1 week | Provider layer + cost tracking |
| 6 | 2-3 weeks | Pipeline + parallel agents |
| 7 | 3-4 weeks | Marketplace + 5 skills (1GB each) |
| 8 | 1 week | Benchmarks |
| 9 | 1-2 weeks | Distribution + landing page |
| 10 | Ongoing | Beta → launch → growth |
| **Total to MVP launch** | **~3 months** | If full-time solo |
| **Total to MVP launch** | **~6 months** | If part-time |
