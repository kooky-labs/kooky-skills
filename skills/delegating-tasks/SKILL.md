---
name: delegating-tasks
description: >
  Analyzes incoming requests and routes them to the appropriate agent(s)
  with clear task definitions, dependency ordering, and handoff sequences.
  Use when a request arrives and needs to be broken down, routed, or
  coordinated across multiple agents. Returns a delegation specification —
  does not execute the work itself.
version: 1.0.0
owner: KAI
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - orchestration
  - delegation
  - workflow
---

# delegating-tasks

Workflow skill for routing requests to the right agent(s) with clear task definitions. Read-only — produces a delegation specification; the actual agent invocation happens downstream.

## What this skill does

Takes an incoming request and returns: the primary agent, any supporting agents, the task definition for each, expected outputs, urgency, and (for multi-agent work) the handoff sequence. Uses the agent routing table below as the canonical capability map.

The skill **plans** the delegation; it does not invoke agents directly. Hand the output to whatever orchestrator actually dispatches work.

## When to use it

- A new request arrives and needs routing to one or more agents.
- A multi-step task requires coordination across agents.
- Determining which agent owns a given capability.
- Breaking down a vague request into discrete agent-owned tasks.

Do **not** use this skill when:

- The requester named a specific agent already — route directly, no need to re-classify.
- The work is implementation by a single known agent — over-spec'ing the delegation adds friction.
- The user's preference is the answer — escalate to approval, not delegation.

## How to use it

### Agent routing table

| Agent | Domain |
|-------|--------|
| HOUDINI (CMO) | Content, social media, brand, marketing strategy |
| MAVERICK (Sales) | Outreach, leads, proposals, pipeline |
| WATSON (QA) | Quality review, security audit, bias detection |
| JARVIS (CTO) | Architecture, code, infrastructure, technical decisions |
| LEONARDO (CPO) | Product strategy, features, user stories |
| SHERLOCK (Research) | Market research, competitive intel, tech evaluation |
| ALFRED (COO) | Operations, process optimization, workflows |
| SCOTTY (Delivery) | Client delivery, milestones, deployment |
| SAM (CS) | Onboarding, support, retention, feedback |
| FINN (CFO) | Financial planning, budgets, pricing |
| TANK (Skills) | Skill curation, installation, marketplace |
| DEWEY (Knowledge) | Knowledge bases, documentation, source evaluation |

### Process

1. **Classify the request:** domain, single vs multi-agent, expected output type.
2. **Select the primary agent** from the routing table.
3. **Identify dependencies** and coordination chains (see common chains below).
4. **Delegate with:** specific task, inputs, expected output, urgency (Now / Today / This Week).
5. **For multi-agent tasks,** specify the handoff sequence — who hands what to whom in which order.
6. **Cap at 3 agents per task** without explicit operator approval — coordination overhead grows fast beyond that.
7. **Always include WATSON in the chain** for client-facing deliverables.

### Common coordination chains

- `SHERLOCK researches → HOUDINI writes content`
- `MAVERICK drafts proposal → WATSON reviews → MAVERICK sends`
- `LEONARDO specs feature → JARVIS architects → SCOTTY delivers`
- `TANK discovers skill → WATSON audits → operator approves install`

## What to escalate

Stop and ask the operator before delegating when:

- The request crosses 3+ agents — confirm scope rather than auto-fanout.
- The required capability isn't in the routing table — escalate before inventing a routing.
- The work needs an agent outside their stated domain — operator decides whether to stretch scope.
- The request is the operator's preference question, not an agent capability question.
- The locale isn't supported (currently `en` only).

## Output format

```
Primary agent:   <CODE>
Supporting:      <CODE, CODE> (empty for single-agent)
Task:            <specific definition>
Inputs:          <what the agent needs>
Expected output: <what "done" looks like>
Urgency:         Now / Today / This Week
Handoff sequence: <agent → agent → agent> (only for multi-agent)
```

## Constraints

- Always delegate — never implement directly from this skill.
- Never assign work outside an agent's stated domain.
- Always specify what "done" looks like for each delegation.
- Limit to 3 agents per task without explicit operator approval.
- Always include WATSON in the chain for client-facing deliverables.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `summarizing-progress` — pairs with this skill for reporting on what delegated work produced
- `convening-council` — different protocol for high-stakes decisions where one agent isn't enough
