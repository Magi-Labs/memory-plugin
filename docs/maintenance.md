# Maintenance and compatibility

## Design

One canonical Agent Skill contains the workflow. Compatibility manifests and configuration templates adapt installation and secret handling to individual clients. There is no proxy, SDK dependency, local database, additional model call, or background process in this repository.

The portable manifest is a skills package. The Claude compatibility manifest also bundles an HTTP MCP entry using Claude's environment references. Codex uses a native bearer-token environment field; Hermes uses its native environment substitution. Other hosts supply credentials through their own authentication facilities.

The package works with the Magi Labs Memory gateway contract. Agent-neutral does not mean every memory service implements those tools. Adaptation to another server belongs at the tool-contract boundary.

## Releasing

Keep `plugin.json`, `.codex-plugin/plugin.json`, and `.claude-plugin/plugin.json` at the same version. Preserve the canonical skill and tool-reference paths. Change client examples when official configuration schemas change.

Create a distributable archive from committed files, outside the repository, including hidden manifests:

```bash
git archive --format=zip --prefix=memory-plugin/ --output=../memory-plugin-0.1.1.zip HEAD
```

Use a reviewed commit/tag for distribution. Do not archive the working directory, client secrets, or caches. This public GitHub repository is separate from a plugin-directory submission.

## Release 0.1.1 — 5 October 2026

- Product homepage and README links use `https://magi-labs.github.io/memory/`.
- Installation docs now reflect the public repository and offer unauthenticated Git cloning.
- All three plugin manifest versions are synchronized. MCP endpoint templates and credential configuration retain their existing deployment-specific behavior.
- No automated tests, manifest validators, client installations or connection flows were run for this metadata/documentation update.

## Release 0.1.0 evidence

- Tool names, signatures, source/document semantics, missing-handoff response, optimistic concurrency, and retry behavior were read from the companion gateway source.
- Client configuration and package layouts were authored using the official sources below, consulted 2026-10-04.
- Installed Codex CLI help confirmed `plugin marketplace add` and `plugin add` syntax.
- Automated tests, manifest validators, actual client installations, live authenticated discovery, and mutation round trips were not run for this release. No existing client settings or production data were changed.

## Official references

- [Agent Skills format](https://agentskills.io/specification)
- [Agent Plugins specification](https://agent-plugins.org/specification)
- [OpenAI plugin packaging](https://developers.openai.com/plugins/build/plugins)
- [Codex configuration](https://learn.chatgpt.com/docs/config-file/config-reference)
- [Claude plugin manifests](https://code.claude.com/docs/en/plugins-reference)
- [Claude plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Claude MCP configuration](https://code.claude.com/docs/en/mcp)
- [Hermes MCP configuration](https://hermes-agent.nousresearch.com/docs/reference/mcp-config-reference)
- [Hermes skills](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills)
- [Companion server source](https://github.com/Magi-Labs/memory)
