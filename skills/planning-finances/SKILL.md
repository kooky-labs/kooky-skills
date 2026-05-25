---
name: planning-finances
description: >
  Produces financial analysis — budgets, revenue projections, cost
  optimization, and pricing strategy — with explicit assumptions,
  sensitivity ranges, and trade-off analysis. Use when a financial
  decision needs data-backed analysis. Not a substitute for
  professional legal or tax advice.
version: 1.0.0
owner: FINN
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - finance
  - planning
  - workflow
---

# planning-finances

Workflow skill for clear financial analysis that enables smart business decisions. Read-only — produces the analysis; never executes financial actions.

## What this skill does

Takes a financial-analysis request (budget review / revenue projections / cost optimization / pricing strategy) plus available data, and returns a structured analysis with: per-scenario ranges (not single-point estimates), explicit assumptions separated from conclusions, sensitivity analysis (what changes if key assumptions are wrong), and trade-off analysis for every recommendation. Conservative bias is the default; optimistic assumptions are flagged.

## When to use it

- Reviewing a budget or analyzing burn rate and runway.
- Creating revenue projections with conservative / base / optimistic scenarios.
- Identifying cost optimization opportunities.
- Evaluating pricing tiers or packaging changes.

Do **not** use this skill when:

- The decision needs professional tax or legal advice — escalate; this skill is not a substitute.
- The data isn't available — gather first; this skill won't fabricate numbers.
- The need is execution (paying invoices, sending receipts) — wrong scope.
- The locale isn't supported (currently `en` only).

## How to use it

1. **Identify the analysis type and gather available data.**
2. **Apply the appropriate framework:**
   - **Budget Analysis** — Revenue by source, costs by category, net + burn rate + runway, observations, recommendations with expected savings.
   - **Revenue Projections** — Conservative / base / optimistic scenarios with stated assumptions, key risks, upside.
   - **Cost Optimization** — Top 5 cost items, necessity / alternatives / optimization per item, ROI for proposed investments.
   - **Pricing Strategy** — Competitive landscape, unit economics (CAC, LTV, LTV/CAC), tier models, "what if we're wrong" scenario.
3. **Always use ranges, not single-point estimates** for projections.
4. **State assumptions explicitly and separately from conclusions.**
5. **Include sensitivity analysis** — what changes if key assumptions are wrong.
6. **For cost cuts, always assess quality + security impact.**
7. **Return the analysis** in the format below.

## What to escalate

Stop and ask the operator before issuing the analysis when:

- The question requires legal, tax, or regulatory expertise.
- The data is missing on multiple assumption inputs — operator decides whether to model anyway with caveats.
- A recommended cost cut would compromise security, compliance, or critical capability.
- The locale isn't supported (currently `en` only).

## Output format

- Structured analysis with tables for comparisons
- Numbers with sources (not rounded estimates without basis)
- Conservative bias with optimistic assumptions flagged
- Trade-off analysis for every recommendation

## Constraints

- Never present projections as certainty.
- Never hide unfavorable numbers.
- Never recommend cost cuts without assessing quality / security impact.
- Never make pricing recommendations without competitive context.
- Escalate legal / tax questions — this skill is not a substitute for professional advice.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
