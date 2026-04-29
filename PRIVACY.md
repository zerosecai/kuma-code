# Kuma Code Privacy Policy

**Last updated:** April 28, 2026

Kuma Code is built local-first. This document describes what data Kuma Code touches, where it goes, and what control you have over it. There is no Kuma-operated cloud gateway between you and your model provider.

## Local-first by default

When Kuma Code runs against a local provider (Ollama on `localhost:11434` or LM Studio on `localhost:1234`), nothing leaves your machine. Your code, prompts, edits, terminal output, and conversation history all stay in process memory and on local disk only.

This is the default install configuration on machines where a local model server is detected.

## Optional cloud burst — opt-in, direct-to-provider

If you choose to enable a cloud provider, the request goes from Kuma Code **directly to that provider's API** (e.g. Ollama Cloud, OpenAI-compatible endpoints). Kuma Code does not proxy requests through any intermediate service we operate. There is no "Kuma servers" hop.

When a cloud provider is active:

- The prompt, the relevant code context, and any tool outputs Kuma decides to include are sent to that provider.
- Whatever the provider does with that data is governed by **their** privacy policy and terms — not ours. Read them before pointing Kuma at a cloud provider.
- API keys are stored in VS Code's secure storage. They are never logged, never sent to anyone except the provider you configured.

## Routing visibility

When Kuma Code's hybrid router decides whether to use a local or cloud model for a given request, it shows you the decision in the UI before sending. You can override per-task ("always cloud" or "always local") or set a project-wide policy. The router never silently sends a private file to a cloud model.

## Telemetry

Kuma Code does not send usage telemetry to us by default. There is no crash-report uploader, no analytics ping, no model-of-the-week reporter.

If we ever add opt-in telemetry, it will be:

- **Off by default.** A clear toggle, not a buried setting.
- **Documented.** Every field that gets sent listed here.
- **Coarse.** No code, no prompts, no file paths.

We will note the change in the release notes and update this document.

## Data residency

Your code lives on your machine. Your machine = your data. If you want a Kuma Code installation that has no possible path to leave a defined network, configure the provider list to local-only and disable cloud providers in settings.

## For enterprise

Kuma Code supports air-gapped operation: a provider configuration with only `ollama-local` or `lm-studio` enabled has no required outbound connections. This is suitable for environments where source code may not leave the corporate network. If you're evaluating Kuma in a regulated context and need a specific data-flow diagram or control mapped out, reach out via the contact below.

## Third-party model providers

When you use a third-party model provider (Ollama Cloud, OpenAI, Anthropic, OpenRouter, etc.), data sent to that provider is governed by **their** privacy policy. We have no visibility into and no control over their data handling. Read their terms before sending production code through their API.

## Changes to this policy

If we change how data flows in Kuma Code, this document is updated and the change is called out in the release notes. The git history of `PRIVACY.md` is the source of truth — `git log PRIVACY.md` shows every revision.

## Contact

For privacy-related questions, contact: **sam@zerosec-ai.com**
