---
name: automating-operations
description: >
  Maps operational workflows, classifies each step (automate fully /
  automate with oversight / human-in-the-loop / cannot automate),
  designs an optimized target-state process, and produces an
  implementation plan with migration + rollback. Use when a process is
  manual, slow, or error-prone and needs automation analysis.
version: 1.0.0
owner: ALFRED
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - operations
  - automation
  - workflow
---

# automating-operations

Workflow skill for designing operational workflows that run smoothly with minimal manual intervention. Read-only — produces the design + plan; never executes the automation.

## What this skill does

Takes a current-state process (manual / slow / error-prone) and returns: a current-state map with steps + handoffs + bottlenecks, a per-step automation classification, a target-state workflow design with triggers + handoffs + completion criteria + error handling, and an implementation plan with migration path + rollback + ownership.

## When to use it

- A manual or slow process needs automation analysis.
- Designing a new workflow from scratch.
- Optimizing an existing workflow with bottlenecks or handoff failures.

Do **not** use this skill when:

- The process isn't documented manually first — you cannot automate what you cannot describe.
- The need is one-off execution, not repeating workflow — wrong scope.
- The decision is whether to automate (operator's call) — this skill answers HOW; the WHETHER is upstream.

## How to use it

1. **Map current state** — document every step, identify manual steps, handoffs, decision points, delays, bottlenecks.
2. **Classify each step:**
   - **Automate fully** — no human judgment needed, deterministic outcome.
   - **Automate with oversight** — agent handles it; human reviews output.
   - **Human-in-the-loop** — requires human judgment or approval.
   - **Cannot automate** — physical action or external dependency.
3. **Design target state** — optimized workflow with triggers, handoffs, completion criteria, and error handling for each step.
4. **Create implementation plan** — what to build, migration path, rollback plan, ownership assignment.
5. **Return the design + plan** in the format below.

## Workflow documentation template

Each workflow should specify: name, owner, trigger, frequency, SLA, and steps with owners and step types (automated / oversight / human / manual).

## What to escalate

Stop and ask the operator before issuing the design when:

- The current state isn't sufficiently documented to map — request more data first.
- An automated step would have material business risk if it fails silently — flag the risk explicitly before recommending the classification.
- The migration would require downtime or coordinated change windows operator hasn't approved.
- The locale isn't supported (currently `en` only).

## Output format

- Current state map with steps, handoffs, bottlenecks
- Automation classification per step
- Target state workflow diagram
- Implementation plan with migration + rollback
- Workflow documentation template populated for the target state

## Constraints

- Never automate something you do not understand manually first.
- Never design a workflow without mapping current state.
- Every step must have defined error handling.
- Always include a human escalation path for edge cases.
- Prefer idempotent operations — running a step twice should not cause harm.
- Every workflow must have a single owner.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
