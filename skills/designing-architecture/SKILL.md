---
name: designing-architecture
description: >
  Designs system architecture, evaluates technical approaches across
  6 dimensions (complexity, cost, speed, scalability, reversibility,
  dependencies), and produces Architecture Decision Records (ADRs).
  Use when making a technical decision that affects system structure,
  choosing between build vs buy, or documenting an architectural
  choice. Designs the system; does not build it.
version: 1.0.0
owner: JARVIS
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - architecture
  - design
  - adr
  - workflow
---

# designing-architecture

Workflow skill for sound technical decisions that balance simplicity, scalability, and speed. Read-only — produces designs and ADRs; the actual implementation is downstream (often via `building-integrations` when the design specifies an MCP integration).

## What this skill does

Takes a technical problem (or change request) plus hard constraints and current-state context. Returns: at least 3 evaluated options, a recommendation with reasoning, and a complete ADR (status / context / decision / options-considered / consequences) ready to commit. The skill enforces "simplest solution that meets requirements" and refuses to over-engineer.

## When to use it

- Designing or changing system architecture.
- Evaluating technical approaches or making build-vs-buy decisions.
- Documenting an architectural decision as an ADR.
- Reviewing an existing architecture for a specific concern (cost, scalability, debt).

Do **not** use this skill when:

- The decision has one obvious answer — over-spec'ing the rationale adds friction.
- The work is pure implementation following an already-decided ADR — use `building-integrations` or a code-authoring skill.
- The "architecture" is actually a personal preference (editor, language, framework) — escalate to operator.
- The problem isn't yet defined — define it first; this skill won't invent the problem.

## How to use it

1. **Define the problem precisely:** what needs to be built or changed, hard constraints (budget, latency, deadline, compliance), current state.
2. **Identify at least 3 options** including "do nothing" when reversibility / sunk-cost considerations make it a real option.
3. **Evaluate each option on 6 dimensions:**
   - **Complexity** — How much does this add to the system?
   - **Cost** — Build + maintain cost over the relevant time horizon.
   - **Speed** — How quickly can this ship?
   - **Scalability** — Does it grow with expected usage?
   - **Reversibility** — How hard is it to change course later?
   - **Dependencies** — What new dependencies (services, vendors, libraries) does this introduce?
4. **Include build-vs-buy analysis** and managed-vs-self-hosted considerations where relevant.
5. **Recommend the simplest solution** that meets requirements. Bias toward managed services and reversible decisions.
6. **Document as an ADR** with: status, context, decision, options considered (with rejection reasoning per option), consequences (positive + negative).
7. **Return the ADR + system design** in the format below.

## What to escalate

Stop and ask the operator before issuing the recommendation when:

- The decision affects shared infrastructure that other teams depend on — coordinate first.
- The recommended option requires budget the operator hasn't approved.
- All 3+ options have equivalent trade-offs — the tie-breaker is operator preference, not architecture.
- The problem definition is ambiguous enough that different reasonable readings produce different ADRs.
- The locale isn't supported (currently `en` only).

## Output format

- **ADR** — Status (Proposed / Accepted / Deprecated / Superseded), Context, Decision, Options Considered (each with rejection reasoning), Consequences (positive + negative)
- **System design** — component diagram, data flow, API contracts, error handling, monitoring approach
- **Clear recommendation** with one-paragraph reasoning

## Constraints

- Every component must justify its existence — simplicity first.
- Prefer managed services over self-hosted when cost is comparable.
- No premature optimization — solve today's problem, design for tomorrow's.
- Never introduce complexity without documenting why.
- Never recommend technology just because it is popular.
- All technical decisions require an ADR before implementation begins.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `building-integrations` — downstream skill that implements MCP-integration designs this skill produces
