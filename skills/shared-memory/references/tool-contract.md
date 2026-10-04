# Magi Labs Memory tool contract

Source: `Magi-Labs/memory`, `app/gateway.py` and `app/storage.py`, inspected at revision `e7d278e` on 2026-10-04. Discover the live schema when connecting; names can be prefixed by the client. The transport is MCP Streamable HTTP. MCP standardizes tool discovery and invocation, not memory tool names.

## Durable source documents

| Tool | Arguments | Meaning |
| --- | --- | --- |
| `search_memories` | `query: string`, `limit: integer = 10` | Relevant indexed results |
| `list_memories` | `page: integer = 1`, `limit: integer = 50` | Source documents and ingestion status |
| `add_memory` | `content: string` | Submit a source; extraction/indexing is asynchronous |
| `get_memory` | `memory_id: string` | Read a source document and its extracted memories |
| `update_memory` | `memory_id: string`, `content: string` | Replace the source content with explicit user intent |
| `delete_memory` | `memory_id: string` | Delete an identified source with explicit user intent |

Add/update content must be nonblank and at most 100,000 characters. Use small useful inputs. Search results can include extracted memory IDs; obtain the corresponding source document ID before reading, updating, or deleting a document. Inspect returned fields rather than assuming the result shape is identical across engine versions. `add_memory` has no exposed idempotency key.

## Task handoffs

| Tool | Arguments |
| --- | --- |
| `list_handoffs` | none |
| `get_handoff` | `project: string`, optional `version: integer` (omit for latest) |
| `save_handoff` | Required `project`, `goal`, `summary`, `expected_version`, `request_id`; optional string arrays `decisions`, `next_steps`, `references` |

An absent handoff returns `{"project":"…","version":0,"context":null}`. A saved handoff includes `project`, `version`, `context`, author, and creation time. Versions are append-only. Same project/request ID and identical payload returns the original saved revision; reusing the ID with a different payload fails. A stale expected version fails rather than overwriting.

Limits:

- Project: 1–100 characters, starts with a letter/digit, remaining characters letters/digits or `._/-`.
- Goal: 1–8,000 characters; summary: at most 20,000 characters.
- Request ID: 8–100 characters, letters/digits or `._-`; a newly generated UUID is suitable.
- Each list: at most 30 items; each item at most 2,000 characters.
- Expected version: integer ≥ 0.

Example payload after reading version 3 (illustrative; generate a fresh request ID for real work):

```json
{
  "project": "owner/project",
  "goal": "Add a theme switcher",
  "summary": "Theme switcher implemented locally. Changes remain uncommitted. Automated tests were not run.",
  "decisions": ["Use system theme until the user chooses a preference"],
  "next_steps": ["Inspect the diff and complete the requested release steps"],
  "references": ["src/components/theme-switcher.tsx"],
  "expected_version": 3,
  "request_id": "e3107d9c-cbb1-4a24-929d-3644487b9170"
}
```

The server labels handoffs with the authenticated client's name. A token grants read or read/write access to the same personal space; project IDs are organizational keys, not permission boundaries.
