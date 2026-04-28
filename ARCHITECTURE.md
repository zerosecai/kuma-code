# ARCHITECTURE.md

> The shape of Kuma Code. Read this before adding/changing any module.
> Changes to this file require explicit user approval.

---

## Module map

```
kuma-code/                                 (monorepo root)
├── packages/
│   ├── kuma-vscode/                       VS Code extension (UI layer)
│   │   ├── src/
│   │   │   ├── extension.ts               Entry point. Wiring only.
│   │   │   ├── ui/
│   │   │   │   ├── sidebar.ts             Main agent sidebar
│   │   │   │   ├── agent-manager.ts       Multi-session manager view
│   │   │   │   └── skills-panel.ts        Installed skills view
│   │   │   └── commands/                  VS Code command handlers
│   │   └── package.json
│   │
│   ├── kuma-core/                         Forked from Kilo. Agent runtime.
│   │   ├── src/
│   │   │   ├── agents/                    Planner / Coder / Reviewer
│   │   │   ├── orchestrator/              Pipeline + parallel scheduler
│   │   │   ├── providers/                 Ollama Cloud / local / LM Studio
│   │   │   ├── tools/                     File / shell / browser tools
│   │   │   └── runtime/                   Session state, artifacts
│   │   └── package.json
│   │
│   └── skill-system/                      OUR IP. The differentiator.
│       ├── src/
│       │   ├── format/                    Types + spec
│       │   ├── loader/                    Discover + load skills from disk
│       │   ├── retriever/                 2-level lookup (topic → section)
│       │   ├── learner/                   Auto-learn from API responses
│       │   └── marketplace/               Skill registry client
│       ├── skills/                        Bundled skills (typescript/react)
│       ├── docs/SKILL_FORMAT.md
│       └── package.json
│
├── scripts/                                Build / release scripts
└── docs/                                   Specs, decisions, reviews
```

---

## Dependency rules

```
kuma-vscode  →  kuma-core  →  skill-system
                    │
                    └─→  providers (external: Ollama, etc.)
```

**Forbidden:**
- `kuma-core` importing from `kuma-vscode` (would break CLI usage)
- `skill-system` importing from `kuma-core` (would break testability)
- Circular imports between packages
- `kuma-vscode` calling providers directly (must go through kuma-core)

If you find yourself wanting to break these rules, the design is wrong. STOP and escalate.

---

## Data flow: a typical task

```
1. User types task in sidebar
       │
       ▼
2. kuma-vscode/sidebar → kuma-core/orchestrator
       │
       ▼
3. orchestrator queries skill-system/retriever:
   "is there a skill match for this task?"
       │
       ├── YES → retrieve content, prepend to prompt → use small local model
       │
       └── NO  → use cloud model (Ollama Cloud / API)
                       │
                       ▼
                   skill-system/learner saves response as draft
       │
       ▼
4. orchestrator runs Plan → Code → Review
       │
       ▼
5. Each stage emits an Artifact → sidebar renders it
       │
       ▼
6. Run persisted to .kuma/runs/<run-id>.json
```

---

## State / persistence

| Where | What | Format |
|---|---|---|
| `.kuma/runs/` | Task run history | JSON, one file per run |
| `.kuma/skills/` | Installed skill packs | Folder per skill |
| `.kuma/cache/` | Model response cache | SQLite |
| `.kuma/config.json` | User config | JSON |
| VS Code settings | Provider keys, defaults | VS Code settings API |

**Never store secrets in `.kuma/`** — those go in VS Code's secure storage.

---

## Provider abstraction

```typescript
interface ChatProvider {
  id: string;
  name: string;
  capabilities: { tools: boolean; streaming: boolean; vision: boolean };
  chat(args: ChatArgs): AsyncIterable<ChatChunk>;
}
```

All providers must implement this interface. Adding a new provider = one file in `kuma-core/src/providers/`.

**MVP providers:**
- `ollama-cloud` (default)
- `ollama-local` (auto-detected)
- `lm-studio` (auto-detected)
- `openai-compatible` (catch-all for other endpoints)

Future: `anthropic`, `openrouter`, `together`, etc. Do NOT add these in MVP.

---

## Skill system contract

```typescript
interface SkillSystem {
  // Phase 1 — already implemented
  loadSkill(dir: string): Promise<LoadedSkill>;
  discoverSkills(rootDir: string): Promise<LoadedSkill[]>;
  rankTopics(skill: LoadedSkill, query: string): SkillTopic[];
  readTopic(skill: LoadedSkill, id: string, section?: string): Promise<RetrievedContent>;

  // Phase 4 — to implement
  recordExample(skill: LoadedSkill, input: LearnInput): Promise<SkillExample>;
  promoteExample(skillId: string, exampleId: string): Promise<void>;

  // Phase 7 — to implement
  searchMarketplace(query: string): Promise<SkillListing[]>;
  installSkill(listingId: string): Promise<LoadedSkill>;
}
```

This contract is **stable**. Changing it requires user approval.

---

## Things that look easy but are NOT

These traps have killed similar projects. Be careful:

1. **Streaming + tool calling at once** — tricky state machine, easy to deadlock
2. **Concurrent skill writes** — auto-learner from N parallel agents = race conditions. Use file locks.
3. **VS Code webview CSP** — strict, rejects most inline scripts. Test in actual VS Code, not just unit tests.
4. **Bun on Windows** — limited support. Test on Windows before claiming Phase 9 done.
5. **Ollama Cloud rate limits** — 429s come without good error messages. Always retry with backoff.
6. **Git worktree corruption** — Kilo uses worktrees for parallel agents. If a worktree gets corrupted, the whole repo can hang. Always use `--force` carefully and test cleanup paths.

If you're touching any of these, switch to Tier 3 mode (propose plan first).
