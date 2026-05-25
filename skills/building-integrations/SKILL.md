---
name: building-integrations
description: >
  Builds MCP (Model Context Protocol) server integrations that connect
  agents to external services — defines tool schemas, implements handlers,
  applies security standards (credentials in env, allowlists, audit
  logging), and writes tests. Use when agents need programmatic access to
  an external API or service. Implements designs from
  `designing-architecture`; produces working integration code.
version: 1.0.0
owner: JARVIS
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - integration
  - mcp
  - infrastructure
  - workflow
---

# building-integrations

Workflow skill for building reliable MCP servers that give agents access to external services. Read-only — produces the integration code, schemas, tests, and deployment docs; the actual deployment is a downstream operator action.

## What this skill does

Takes an external service + required actions + consuming-agent context + auth method, and returns: tool schemas (strict JSON Schema), handler implementations, error handling (with retry / timeout / structured errors), security configuration (env-var credentials, allowlists, audit logging), tests (unit + integration), and deployment documentation.

The skill applies KOOKY's MCP integration standards consistently: lowercase-hyphen tool names, agent-self-correcting error messages, exponential-backoff retries (1s/2s/4s, max 3), 30s default timeout, env-var credentials only, no raw API errors leaked, allowlist-only URL access.

## When to use it

- Connecting agents to a new external API or service.
- Defining MCP tool schemas and implementing handlers.
- Reviewing or improving an existing MCP integration for security / reliability gaps.
- Adding a new tool to an existing MCP server.

Do **not** use this skill when:

- The architecture decision hasn't been made yet — run `designing-architecture` first.
- The integration is one-off and won't be reused — a single CLI script is lighter weight than an MCP server.
- The external service is volatile (frequent breaking changes) — MCP server has maintenance cost; consider on-demand `curl` first.
- The agent needs a stateful interactive session — MCP tool calls are request/response; sessions need a different surface.

## How to use it

1. **Identify the external service, required actions, consuming agents, and authentication method.**
2. **Design each tool with:**
   - **Name** — lowercase-hyphens
   - **Description** — clear enough for an agent to know when to use it
   - **inputSchema** — strict JSON Schema with required + optional fields
   - **Handler function** — implements the action
3. **Implement error handling:**
   - Catch all external API errors
   - Return structured error messages that help agents self-correct (e.g., `"Missing required parameter: platform. Expected one of: x, linkedin, tiktok"` — not raw stack traces)
   - Retry transient failures with exponential backoff: 1s, 2s, 4s, max 3 retries
   - Default timeout: 30 seconds
4. **Apply security standards:**
   - Credentials in environment variables (never hardcoded)
   - Input validation on every tool
   - Least-privilege access (don't request scopes the integration doesn't need)
   - Audit logging (tool name + timestamp + success/failure — **not** the input data)
   - URL allowlist for any HTTP calls
5. **Write tests:**
   - Unit tests per handler
   - Integration tests with mocks for the external service
   - Error case coverage (auth failure, rate limit, timeout, invalid input)
6. **Document deployment + configuration** — environment variables, secret-store setup, expected runtime, scaling notes.
7. **Return the integration package** (file tree + content) in the format below.

## Project structure

```
server.py         # Main server entrypoint
tools/            # One file per tool or tool group
schemas/          # Tool parameter schemas
tests/            # Unit + integration tests
README.md         # Deployment + configuration docs
```

## What to escalate

Stop and ask the operator before producing the integration when:

- Authentication requires OAuth setup the operator hasn't approved.
- The external service's TOS prohibits programmatic access — surface, do not build.
- The integration needs to write to systems with elevated risk (production DB, payment processor, deletion endpoints) — confirm scope explicitly.
- The required external service isn't on a known allowlist — operator decides.
- The locale isn't supported (currently `en` only).

## Output format

- File tree of the integration package (server.py / tools/ / schemas/ / tests/ / README.md)
- File contents per file, in order
- Configuration spec — env vars, secrets, allowlisted URLs
- Test execution instructions
- Deployment notes

## Constraints

- All external URLs must be documented and allow-listed.
- Never expose raw API errors or stack traces to agents.
- Error messages must help agents self-correct.
- All tools should be idempotent where possible.
- Never allow tools to modify agent configurations.
- Never build arbitrary URL fetching without an allowlist.
- All new MCP tools require security review before production deployment.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- SPEC v1.2 §3.4 — connector-script error handling pattern (capture-then-check, not bare `curl | jq`)
- `designing-architecture` — upstream skill that produces the ADR + design this skill implements
- `auditing-skill-security` — downstream gate that audits MCP integrations before install / deployment
