---
name: qualifying-leads
description: >
  Evaluates and scores incoming leads against ICP criteria across 5 weighted
  dimensions, assigns a tier (Hot / Warm / Cool / Cold), and recommends
  specific next actions. Use when a new lead arrives and needs qualification
  before sales effort is allocated. Pairs with `conducting-outreach` — the
  qualification report's prospect_data + tier feed the outreach drafting.
version: 1.0.0
owner: MAVERICK
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - sales
  - qualification
  - workflow
---

# qualifying-leads

Workflow skill for scoring leads quickly and accurately so sales effort goes to the highest-probability prospects. Read-only — produces a qualification report; does not invoke outreach.

## What this skill does

Takes available lead data and returns a structured qualification report: weighted score per dimension, total weighted score, tier classification, recommended next actions (primary + secondary), key observations, and information gaps. Optionally flags competitor mentions for downstream competitive-intelligence routing.

The qualification output is the input to `conducting-outreach` — the tier shapes message intensity, the prospect data shapes personalization, and the key observations shape opening lines.

## When to use it

- A new lead comes in from any source (inbound form, LinkedIn, referral, content download, event).
- Re-qualifying a lead after new information emerges.
- Deciding how much sales effort to allocate to a prospect.
- Triaging a batch of leads to rank by priority.

Do **not** use this skill when:

- The lead is already qualified and you need outreach — use `conducting-outreach` directly.
- Data is too sparse to score meaningfully — request more data first; don't guess.
- The "lead" is actually a known existing customer — different workflow.

## How to use it

1. **Gather available data:** name, title, company, industry, size, engagement history, stated need, source channel.
2. **Score on five weighted dimensions** (1-5 per dimension):
   - **Profile Fit (30%)** — Match to ICP segments (5 = actively seeking; 1 = no fit).
   - **Intent Signal (25%)** — Explicit interest level (5 = asked directly; 1 = no signal).
   - **Budget Likelihood (20%)** — Ability to pay (5 = indicated budget; 1 = no indicators).
   - **Timing (15%)** — Urgency (5 = active transition now; 1 = no timing signal).
   - **Engagement Quality (10%)** — Interaction depth (5 = thoughtful replies; 1 = no response).
3. **Calculate weighted score and assign tier:**
   - **Hot (4.0-5.0)** — Personal outreach within 4 hours.
   - **Warm (3.0-3.9)** — Personalized outreach within 24 hours, add to nurture.
   - **Cool (2.0-2.9)** — Content nurture, monitor for intent signals.
   - **Cold (1.0-1.9)** — Archive, no active pursuit.
4. **Generate specific next-action recommendations** — primary (the immediate move) + secondary (the next move after).
5. **Flag competitor mentions** for competitive-intelligence routing.
6. **Surface information gaps** explicitly — what data was missing and how to fill it.

## What to escalate

Stop and ask the operator before issuing the qualification when:

- Data is missing on 3+ of the 5 dimensions — operator decides whether to score with caveats or pause.
- The lead appears to be a competitor, partner, or existing customer (mis-routed).
- The lead's request requires immediate operator attention (e.g., enterprise inquiry, legal threat, security disclosure) — surface, don't auto-tier.
- The locale isn't supported (currently `en` only).

## Output format

- Qualification report with weighted score per dimension
- Tier classification (Hot / Warm / Cool / Cold)
- Recommended actions (primary + secondary)
- Key observations (3-5 bullets)
- Information gaps (explicit list of missing data)
- Competitor mentions (when present)

## Constraints

- If data is insufficient for a dimension, mark "insufficient data" rather than guessing.
- Never inflate scores to make a lead look better than it is.
- Always recommend a specific next action, even for cold leads.
- No lead should sit unqualified for more than 24 hours.
- The 5-dimension weights are policy — operator can adjust per campaign; document the adjustment in the report when it deviates.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `conducting-outreach` — consumes this skill's output (qualification_tier + prospect_data + key_observations)
