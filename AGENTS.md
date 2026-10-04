# Maintaining Memory Plugin

## Shared maintainer guidelines

Authorized maintainers use [Magi Labs standards](https://github.com/Magi-Labs/standards), the private source for shared repository, label, architecture, UI and documentation guidelines.
Reviewed standards revision: `f12c3725943f09dcabaaa341dd3d06ef26b6d4c9`.
Read the relevant policies when accessible, then follow the project requirements below. If private access is unavailable, use these local instructions and report that limitation; public contributions do not require private access.

## Project requirements

- Keep the canonical workflow in `skills/shared-memory/`; it must not depend on one agent vendor, a personal hostname, or local credentials.
- Keep client configuration differences in `adapters/`, installation docs, and compatibility manifests.
- Follow the actual tool schemas exposed by Magi Labs Memory. Do not invent tools or claim every MCP memory server uses this contract.
- Keep manifest identity and versions synchronized. Package only tracked source files.
- Preserve the distinction between user statements, extracted/inferred facts, and task handoffs.
- Never commit credentials, personal memories, exports, client configuration, or provider secrets.
- Do not auto-install into a user's agents or change production while editing this repository.
- Run or add tests only when the user asks. Record unverified client behavior honestly.
