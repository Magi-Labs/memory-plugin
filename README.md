# Memory Plugin

**Companion repository:** [Memory](https://github.com/Magi-Labs/memory) — deploy the personal memory server, dashboard, and MCP gateway used by this plugin.

One personal memory workflow across agents and devices. Remember useful facts, retrieve relevant context, and continue work from versioned handoffs.

Point it at your own deployment. The shared skill has no fixed hostname, model provider, or agent dependency.

## Install

Two parts are needed: **the skill/plugin** teaches the workflow; **the MCP connection** provides authenticated access to your server. Installing a skill does not grant access to memories.

| Client | Package | Connection guide |
| --- | --- | --- |
| Codex | Plugin marketplace + shared skill | [Codex](docs/install.md#codex) |
| Claude Code | Plugin marketplace + shared skill + native MCP entry | [Claude Code](docs/install.md#claude-code) |
| Hermes | Copy the same Agent Skill into the active profile | [Hermes](docs/install.md#hermes) |
| Other Agent Skills / MCP clients | Same skill folder + native remote MCP settings | [Generic clients](docs/install.md#other-agents) |
| ChatGPT web / OAuth-only connectors | Requires additional server authentication integration | [Current limits](docs/install.md#web-and-oauth-only-clients) |

For Codex:

```bash
codex plugin marketplace add Magi-Labs/memory-plugin
codex plugin add memory-plugin@magi-memory
```

For Claude Code, inside the client:

```text
/plugin marketplace add Magi-Labs/memory-plugin
/plugin install memory-plugin@magi-memory
```

Then follow [connection setup](docs/install.md). This repository is private; cloning and marketplace installation require GitHub access. Other agents can use an authenticated local clone of `skills/shared-memory/` without a plugin marketplace.

## Use it

- “Remember that I prefer React with shadcn and dark mode.”
- “Recall the decisions for owner/project.”
- “Save a handoff for owner/project before I switch agents.”
- “Resume owner/project using the latest handoff.”

Hosts decide when skills are selected. Explicitly invoke `shared-memory` using your host's skill picker if it is not selected automatically. Consistent start-of-task retrieval can be enabled with the optional instruction in [the install guide](docs/install.md#optional-task-start-instruction).

## What is shared

Durable facts use the server's memory backend. Handoffs preserve the goal, current state, decisions, next steps, and references without another extraction LLM. The client package itself has no runtime dependency or LLM call and does not copy chat history. Model/provider choices stay on the server.

MCP is portable, but memory tool schemas and client authentication formats differ. This package targets the [Magi Labs Memory tool contract](skills/shared-memory/references/tool-contract.md); other servers need compatible tools or an adapter. Client setup examples are not evidence of successful end-to-end installation in every agent.

## Repository layout

```text
skills/shared-memory/       Canonical portable workflow and tool reference
plugin.json                 Agent Plugins 1.0 skills package
.codex-plugin/              Codex compatibility manifest
.agents/plugins/            Codex marketplace catalog
.claude-plugin/             Claude Code manifest and marketplace
adapters/                   Native MCP configuration templates
docs/                       Installation, compatibility, maintenance
```

The portable package intentionally leaves MCP authentication to the host. Agent Plugins 1.0 does not define arbitrary environment interpolation for remote headers; bundling one vendor's secret syntax would break portability. Claude's compatibility manifest uses its documented environment substitution; Codex and Hermes use the provided native configuration.

## Status

Version 0.1.0. Authored against the existing gateway source and official client documentation. Client installs, authenticated tool discovery, and memory mutation flows have **not** been exercised for this release. No automated tests were run. See [maintenance and sources](docs/maintenance.md).
