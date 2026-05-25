---
name: managing-customer-success
description: >
  Manages client onboarding (Day 1-30 lifecycle), support triage (P1-P4
  SLAs), health checks, and churn intervention. Use when onboarding a
  new client, handling a support request, running a health check, or
  detecting and intervening on churn risk.
version: 1.0.0
owner: SAM
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - customer-success
  - support
  - retention
  - workflow
---

# managing-customer-success

Workflow skill for ensuring clients get value from their investment and stay long-term. Read-only — produces onboarding plans, support responses, health checks, churn interventions; does not transact with billing or contracts.

## What this skill does

Takes a customer-success context (new onboarding / support ticket / scheduled health check / churn signal detected) and returns: phase-appropriate onboarding plan, prioritized support response, health-check report with risks + opportunities, or churn-intervention plan with 2-3 options for the client.

## When to use it

- Onboarding a new client (Day 1-30 lifecycle).
- Triaging and responding to a support request.
- Running a client health check or satisfaction review.
- Detecting and intervening on churn signals.

Do **not** use this skill when:

- The need is project delivery — use `managing-delivery`.
- The decision is contractual (renewal pricing, scope expansion contract) — escalate to operator.
- The interaction is with someone who is not a current client — wrong scope.

## How to use it

### Onboarding (Day 1-30)

1. **Day 0** — Send welcome message, set expectations, share getting-started guide.
2. **Day 1-3** — Verify technical setup and first agent interactions.
3. **Day 3-7** — Guide client to first tangible result. Target time-to-value under 7 days.
4. **Day 14** — Check in on friction and feedback.
5. **Day 30** — Success review, set ongoing criteria, establish cadence.

### Support triage

Classify by priority and respond within SLA:

- **P1** (service down) — 1-hour response, escalate immediately.
- **P2** (broken feature, workaround exists) — 4-hour response.
- **P3** (question / how-to) — 24-hour response, self-serve first.
- **P4** (feature request) — 48-hour response, log for product.

### Churn intervention

Watch for: usage drop >30% week-over-week, no login 7+ days, unresolved tickets, negative sentiment. Reach out with genuine concern, listen, present 2-3 options.

## What to escalate

Stop and ask the operator before acting when:

- A P1 issue requires operator-level escalation (executive contact at client, regulatory implication).
- A churn intervention would require contractual or pricing concession beyond normal authority.
- A support response would commit to a feature timeline that needs product / engineering confirmation.
- The client signals a complaint that may have legal / reputational implications.
- The locale isn't supported (currently `en` only).

## Output format

- **Health check** — Usage, satisfaction, risks, opportunities, next actions
- **Support response** — Acknowledgment, solution, timeline, follow-up offer
- **Churn intervention** — Warning signals observed, intervention plan with 2-3 options

## Constraints

- Always follow up after resolution to confirm satisfaction.
- Never use templates without personalization.
- Never share one client's information with another.
- Never make promises about features or timelines without checking with product / engineering.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `managing-delivery` — upstream handoff at go-live (delivery phase → CS phase)
