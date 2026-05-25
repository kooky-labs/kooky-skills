---
name: managing-delivery
description: >
  Tracks client project delivery across 4 phases (kickoff, execution,
  QA, go-live), manages milestones, coordinates agent handoffs, and
  ensures work ships on time, on spec, and on quality. Use when
  managing a client project from sales-handoff through go-live and
  customer-success handoff.
version: 1.0.0
owner: SCOTTY
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - delivery
  - project-management
  - workflow
---

# managing-delivery

Workflow skill for getting client work delivered on time, on spec, and on quality. Read-only — produces trackers, status updates, handoff documents; does not execute the underlying delivery work.

## What this skill does

Takes a delivery context (new project / mid-execution / approaching go-live) and returns: a project tracker with milestones and owners, blocker report with impact and ETA, risk register, and handoff documents at phase boundaries. The skill enforces the 4-phase model (kickoff → execution → review/QA → go-live/handoff) and the cross-phase handoff protocols.

## When to use it

- A new client project kicks off (handoff from sales).
- Tracking progress against milestones during execution.
- Coordinating handoffs between agents on a delivery.
- Planning go-live and handoff to customer success.

Do **not** use this skill when:

- The work is not a client project (internal-only) — different cadence; use a lighter tracker.
- The need is the agent work itself (designing, building, reviewing) — this skill coordinates, doesn't produce.
- The locale isn't supported (currently `en` only).

## How to use it

### Four delivery phases

1. **Kickoff** — confirm scope, deliverables, and timeline. Create project tracker. Assign agents. Set communication cadence.
2. **Execution** — track daily progress against milestones. Coordinate between agents. Resolve dependency blocks immediately. Run quality checks.
3. **Review & QA** — submit deliverables for quality review (use `reviewing-quality`). Iterate on feedback. Manage client review rounds. Final QA before go-live.
4. **Go-Live & Handoff** — deploy / deliver. Client walkthrough. Hand off to customer success (use `managing-customer-success`). Post-delivery retrospective.

### Handoff protocols

- **From Sales** — receive client details, agreed scope, timeline, requirements, contract / pricing.
- **To Customer Success** — provide what was delivered, known issues, client preferences, renewal timeline, expansion opportunities.

## What to escalate

Stop and ask the operator before acting when:

- A blocker has no clear owner — surface immediately; escalation is part of the job.
- A timeline slip is material (>10% of remaining timeline) — operator decides whether to extend, cut scope, or communicate.
- The client requests a scope change that the agents executing don't have capacity for — operator approves or pushes back.
- Quality review (via `reviewing-quality`) returns FAIL on a deliverable — operator decides whether to iterate or escalate further.
- The locale isn't supported (currently `en` only).

## Output format

- Project tracker — milestones, owners, due dates, status, notes
- Blockers — impact, owner, ETA
- Risks — likelihood, impact, mitigation
- Next actions — owners + deadlines
- Phase-handoff document at each phase boundary

## Constraints

- Surface blockers immediately — never hide delays.
- Every blocker must have an owner and an ETA.
- Never promise a timeline without checking with executing agents.
- Never accept scope changes without updating the timeline.
- Every deliverable gets quality review before the client sees it.
- Status reporting: daily (internal), weekly (client).

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `reviewing-quality` — quality gate this skill submits deliverables to
- `managing-customer-success` — downstream handoff target at go-live
