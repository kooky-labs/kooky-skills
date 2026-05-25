---
name: designing-products
description: >
  Defines features, prioritizes backlogs (ICE framework), writes user
  stories with acceptance criteria, and designs user flows. Use when
  making product decisions — scoping a feature, prioritizing work, or
  validating a problem. Returns size estimates (small/medium/large),
  not time estimates.
version: 1.0.0
owner: LEONARDO
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - product
  - design
  - workflow
---

# designing-products

Workflow skill for product decisions that maximize user value with minimal complexity. Read-only — produces specs, stories, flows; never builds.

## What this skill does

Takes a product question (feature, scope change, backlog prioritization, user-flow design) and returns: a validated problem statement, 2-3 solution approaches with the smallest shippable version of each, explicit must-have / should-have / won't-have scope, success criteria, user stories with acceptance criteria, and (when prioritizing) an ICE-framework rationale.

## When to use it

- Defining a new feature or scoping a product change.
- Prioritizing a backlog of work.
- Writing user stories with acceptance criteria.
- Designing user flows (happy path, errors, edge cases).

Do **not** use this skill when:

- The problem isn't validated yet — validate first; don't define features for invented problems.
- The work is implementation — wrong scope; this skill specs, doesn't build.
- The decision is the operator's preference (visual / brand) — escalate.

## How to use it

1. **Validate the problem** — pain point, evidence, size, cost of inaction.
2. **Explore solutions** — generate 2-3 approaches; evaluate the smallest shippable version of each.
3. **Define scope** — Must have / Should have / Won't have (this version). Always name exclusions explicitly.
4. **Set success criteria** — what metric moves, by how much, over what timeline.
5. **Write user stories** — "As a [user], I want to [action], so that [outcome]" with acceptance criteria, out-of-scope items, dependencies, and size (small / medium / large — never time estimates).
6. **Prioritize (if applicable)** — ICE framework: Impact (1-10) × Confidence (1-10) × Ease (1-10) / 10.
7. **Design user flows (if applicable)** — happy path first, then decision points, error states, entry / exit points.

## What to escalate

Stop and ask the operator before issuing the spec when:

- The problem hasn't been validated and you can't produce the evidence.
- The scope crosses a boundary the operator owns (legal, brand, strategic positioning).
- Two or more approaches are equally valid — operator's call.
- The locale isn't supported (currently `en` only).

## Output format

- Problem statement (never skip this)
- Proposed solution with explicit scope (must-have / should-have / won't-have)
- User stories with acceptance criteria
- Prioritization rationale if multiple items (ICE scores)
- Dependencies and risks
- Size estimate (small / medium / large; **never** time estimates)

## Constraints

- Never build a feature without a validated problem.
- Never define scope without naming what is excluded.
- Never write acceptance criteria that cannot be tested.
- Never prioritize based on who is asking loudest — use the framework.
- Never give time estimates — use size sizing (small / medium / large).

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
