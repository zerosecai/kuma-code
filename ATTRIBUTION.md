# Attribution

## Kuma Code's lineage

Kuma Code is a fork in a chain of MIT-licensed agentic coding tools. Each ancestor contributed something we kept and built on. This file is the long form of the credits — please read it before assuming any feature in Kuma Code is "ours."

The chain, newest to oldest:

```
Kuma Code  →  Kilo Code  →  Roo Code  →  Cline
```

---

## Direct ancestor: Kilo Code

**Repository:** https://github.com/Kilo-Org/kilocode
**License:** MIT

Kuma Code is a direct fork of Kilo Code, taken from `main` and re-anchored. Almost everything you see at the platform level — the VS Code extension scaffold, the Tauri/Electron desktop wrappers, the JetBrains plugin, the multi-package monorepo layout, the build pipelines, the i18n system, the agent runtime, the tool integrations (file, shell, browser, MCP), the indexing engine, the SDK — comes from Kilo. They built the running engine. Kuma replaces the panels, the brand, and the routing layer; the chassis underneath is theirs.

Specific things we inherit unchanged or near-unchanged from Kilo at the time of this fork:

- The agent runtime in `packages/opencode` (which itself is a fork of OpenCode).
- The VS Code extension structure and webview architecture in `packages/kilo-vscode`.
- The Agent Manager multi-session UI.
- The provider abstraction surface (which we extend rather than replace).
- The skill loader contract — though our 1 GB skill packs and 2-level retriever are new.

If you contribute upstream-relevant fixes to Kuma Code that aren't Kuma-specific, please consider sending them to Kilo too.

## Grandparent: Roo Code

**Repository:** https://github.com/RooVetGit/Roo-Code
**License:** MIT

Kilo Code is itself a fork of Roo Code. Roo's contributions that flow downstream into Kuma include the early agent orchestration patterns, the custom-modes system (which we rename in our UI to Planner / Coder / Reviewer but is the same machinery underneath), and the chat / tool-call format that the runtime still speaks. Roo took Cline's agentic loop and built a polished extension experience around it.

## Original: Cline

**Repository:** https://github.com/cline/cline
**License:** MIT

Cline is where the agentic coding loop in this lineage was born — the basic pattern of "give the model tools, let it plan, read results, iterate until done" that every fork still uses at its core. The tool-execution model, the permission-prompt UX, the early file/shell tools — they're Cline's. Without Cline, there's no Roo, no Kilo, and no Kuma.

---

## What Kuma Code adds

Things that aren't in any upstream and are part of the Kuma diff:

- **1 GB modular skill packs.** Format spec, loader, 2-level table-of-contents retriever, auto-learner.
- **Three-agent pipeline.** Planner / Coder / Reviewer as a first-class orchestrator, with up to ten parallel coders on the roadmap.
- **Hybrid Ollama Cloud + local Ollama + LM Studio** as the default provider matrix, with rules-based routing between them.
- **32 GB RAM / 4 TB SSD developer workstation** as the explicit hardware target and tuning baseline.
- **TypeScript + React + Vite first-class support**, including the seed skill pack used to dogfood the runtime.
- **Brand and identity**: cyber-bear logo, BRAND_GUIDE, palette, wordmark.

The Kuma source diff against upstream Kilo at the time of this writing is small — most of the visible difference is metadata, brand assets, and the skill-system package scaffold. Larger structural changes are tracked in the repository's roadmap and will land in subsequent commits.

## License

All upstream code (Kilo Code, Roo Code, Cline, OpenCode, plus their respective dependencies) remains under its original MIT license. Our additions on top are also MIT. The original copyright notices are preserved in [LICENSE](LICENSE) as the MIT terms require, alongside our own.

If you redistribute Kuma Code, the MIT license requires you to keep these notices intact.

## Thank you

Open source forks live or die on the goodwill of the projects they're forked from. Cline, Roo Code, and Kilo Code chose MIT and chose to build in public. We're grateful for that, and we try to repay it by being explicit about what we took, what we changed, and where we got it from.
