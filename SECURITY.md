# Security Policy

## Reporting a vulnerability

If you've found a security issue in Kuma Code, please report it privately so we can fix it before it's public.

**Preferred contact (interim):** sam@zerosec-ai.com
**Permanent address (when domain registration completes):** security@kumacode.dev — *not yet active; use the interim address until further notice*

When you report, please include:

- A clear description of the issue and where it lives in the codebase.
- Reproduction steps (the smaller the repro, the faster the fix).
- The version of Kuma Code, the OS, and the LLM provider configuration you were running.
- Any proof-of-concept code or output (sanitized if it leaks anything sensitive).

Please **do not** open a public GitHub Issue for security vulnerabilities. Private email first.

## What we will do

| When | What |
|---|---|
| Within 24 hours | Acknowledge receipt of your report. |
| Within 7 days | Initial assessment: confirm or disprove, classify severity, give you our intended timeline. |
| Throughout | Keep you updated as the fix progresses. |
| At fix release | Credit you in the release notes (unless you prefer to stay anonymous). |

We follow a **coordinated disclosure** model. We ask that you give us a reasonable window to ship a fix before publishing details. In return we won't sit on the issue.

## AI-generated reports

We do not accept AI-generated security reports. The signal-to-noise ratio is poor and we don't have the bandwidth to triage them. A report that looks like an LLM output (vague phrasing, hallucinated CVE numbers, unverified PoCs) will be closed without response. A report from a human who used AI as one of many tools and verified the issue themselves is welcome.

## Scope

The following are **in scope** for vulnerability reports:

- The Kuma Code VS Code extension (`packages/kilo-vscode`).
- The agent runtime (`packages/opencode`).
- The skill system (`packages/skill-system`) — loader, retriever, marketplace client.
- Any official Kuma Code distribution artifact (`.vsix`, signed binaries, brand-domain downloads).

## Out of scope

The following are **not** vulnerabilities in Kuma Code itself:

| Category | Why |
|---|---|
| Third-party model providers | Data sent to Ollama Cloud, OpenAI, Anthropic, OpenRouter, etc. is governed by their security and privacy policies. Report to them. |
| External MCP servers | MCP servers you configure are outside our trust boundary. |
| Self-configured server mode | If you start Kuma's CLI server mode without a password and expose it, that's expected behavior, not a Kuma vulnerability. |
| User configuration files | A user editing their own config to do dangerous things is not an attack vector. |
| Sandbox escapes | Kuma Code does not sandbox the agent. The permission system is a UX safeguard, not an isolation boundary. If you need true isolation, run Kuma inside a Docker container or VM. |
| Upstream Kilo / Roo / Cline issues | Report those to the respective project. We will pick up the fix on the next upstream sync. |

## A note on the agent's permissions

Kuma Code can read and write files, run shell commands, and (when configured) execute network requests. The permission-prompt UX exists to keep you aware of what the agent is doing, **not** to sandbox it. Don't run Kuma against a code path you wouldn't trust a human dev with.

## Past advisories

There are no published advisories yet. When we ship one it will appear here and in the GitHub Security Advisories tab.
