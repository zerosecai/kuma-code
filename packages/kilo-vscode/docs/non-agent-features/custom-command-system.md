# Custom Command System

**Priority:** P2

## Remaining Work

- Slash command input handling in chat (detect `/` prefix, show command list)
- Project-level command discovery (scan `.kuma-code/commands/` or similar)
- YAML frontmatter metadata support
- Symlink-aware command discovery
- VS Code command palette entry points
- Wire to CLI's custom command system for execution

## Primary Implementation Anchors (kuma-code-legacy)

These exist in the [kuma-code-legacy](https://github.com/Kilo-Org/kuma-code-legacy) repo, not in this extension:

- `src/services/command/`
