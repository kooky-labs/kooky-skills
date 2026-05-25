# kooky-skills

A public showcase of skills authored for Claude-based agent fleets. Demonstrates skill design discipline: narrow scope, automated compliance checking, role-based ownership, operator-centric documentation.

This repository is **illustrative**, not production. Skills here may change, get refactored, or be superseded. It's a snapshot of how I work with skills — not the operational system any live fleet runs on.

All skills in this repository conform to **SPEC v1.2** — the standardized skill contract used by KOOKY-authored skills.

---

## Skill structure

Each skill lives under `skills/<skill-name>/` and follows this layout:

```
skills/
  <skill-name>/
    SKILL.md            # required — 9-field YAML frontmatter + operator-centric body (§2.3)
    interface.yaml      # required — machine-readable capability surface (exposes/consumes/flags)
    PILOT_LEARNINGS.md  # optional — findings from refactor, only when surfacing new SPEC gaps
    scripts/            # optional — connector scripts (one file per script, no extension)
    references/         # optional — supplementary docs / examples
```

`SKILL.md` frontmatter (SPEC v1.2 — 9 required fields):

```yaml
---
name: skill-name
description: >
  Multi-line description of what the skill does and when to use it.
  Cross-references peer skills where boundaries matter.
version: 1.0.0
owner: AGENT_CODE              # owning agent from agent-mapping.json
status: active                 # active / deprecated / experimental
last_modified: YYYY-MM-DD
blast_radius: read-only        # read-only / writes-local / writes-external
locale_support:
  - en
requires: []                   # runtime capabilities / dependencies (W5)
tags:
  - tag1
  - tag2
---
```

Body follows the four-section operator-centric convention (SPEC §2.3): **What this skill does / When to use it / How to use it / What to escalate.**

---

## Pre-installed skills

Twenty skills mapped to thirteen agent roles. Each is narrowly scoped, SPEC v1.2-compliant, and audit-ready. Adapt to your own fleet.

| Skill | Owner | Role | Description |
|---|---|---|---|
| [creating-social-content](skills/creating-social-content/SKILL.md) | HOUDINI | CMO | Draft platform-native social media posts (X, LinkedIn, TikTok, Instagram) in founder voice |
| [copywriting](skills/copywriting/SKILL.md) | HOUDINI | CMO | Draft marketing copy: headlines, body, CTAs, email subject lines, ads, taglines |
| [planning-content](skills/planning-content/SKILL.md) | HOUDINI | CMO | Plan content calendars across pillars (week / month / quarter) |
| [conducting-outreach](skills/conducting-outreach/SKILL.md) | MAVERICK | Sales | Draft personalized LinkedIn / email outreach + follow-up sequences |
| [qualifying-leads](skills/qualifying-leads/SKILL.md) | MAVERICK | Sales | Score leads across 5 weighted dimensions; assign Hot/Warm/Cool/Cold tier |
| [reviewing-quality](skills/reviewing-quality/SKILL.md) | WATSON | QA | Review deliverables (content / code / agent output / business material) on 5 dimensions |
| [auditing-skill-security](skills/auditing-skill-security/SKILL.md) | WATSON | QA | Runtime security audit (prompt injection / exfiltration / scope / code safety / Tier-B extras) |
| [delegating-tasks](skills/delegating-tasks/SKILL.md) | KAI | Orchestrator | Classify requests, route to right agent(s), define handoff sequence |
| [summarizing-progress](skills/summarizing-progress/SKILL.md) | KAI | Orchestrator | Generate daily / weekly / project-status executive reports |
| [convening-council](skills/convening-council/SKILL.md) | KAI | Orchestrator | Multi-agent decision protocol with adversarial counter-pass; writes one decision row |
| [designing-architecture](skills/designing-architecture/SKILL.md) | JARVIS | CTO | Evaluate technical approaches on 6 dimensions; produce ADRs |
| [building-integrations](skills/building-integrations/SKILL.md) | JARVIS | CTO | Build MCP server integrations with schemas, handlers, security, tests |
| [designing-products](skills/designing-products/SKILL.md) | LEONARDO | CPO | Define features, prioritize backlog (ICE), write user stories, design flows |
| [automating-operations](skills/automating-operations/SKILL.md) | ALFRED | COO | Map workflows, classify automation potential, design target state + implementation plan |
| [researching-skills](skills/researching-skills/SKILL.md) | SHERLOCK | Research | Discover candidate skills externally; pre-screen for fit + security |
| [planning-finances](skills/planning-finances/SKILL.md) | FINN | CFO | Budget analysis, revenue projections (conservative/base/optimistic), cost optimization, pricing |
| [managing-skills](skills/managing-skills/SKILL.md) | TANK | Skills | Lifecycle management of installed skills — install / audit / monitor / remove |
| [managing-delivery](skills/managing-delivery/SKILL.md) | SCOTTY | Delivery | Track client projects across 4 phases (kickoff / execution / review-QA / go-live) |
| [managing-customer-success](skills/managing-customer-success/SKILL.md) | SAM | CS | Onboarding (Day 1-30), support triage (P1-P4 SLAs), health checks, churn intervention |
| [managing-knowledge](skills/managing-knowledge/SKILL.md) | DEWEY | Knowledge | Source evaluation (5-dim weighted), structure audits, maintenance cadence, deprecation |

See [`skills/agent-mapping.json`](skills/agent-mapping.json) for the canonical owner → skills mapping.

---

## Compliance checking

Every skill in this repository passes the automated SPEC v1.2 compliance gate. The compliance checker validates SPEC §8 items 6-11 mechanically:

- **Item 6** — `blast_radius` declared honestly (matches what the skill actually does)
- **Item 7** — `requires` declares runtime dependencies + environment capabilities
- **Item 8** — body covers the four operator-centric sections (what / when / how / escalate)
- **Item 9** — no hardcoded user-facing strings that should live in `i18n/`
- **Item 10** — no conflicting capability claims across the skill set (requires `CAPABILITIES.md` for full check)
- **Item 11** — connector scripts use the canonical §3.4 capture-then-check pattern (no bare `curl | jq` line-continuation)

Items 1-5 are manual-review only (intent / authenticity / scope / naming / authorship).

---

## Contributing

1. Fork this repo
2. Copy `skills/_template/` to `skills/<your-skill-name>/`
3. Fill in `SKILL.md` (9-field frontmatter + operator-centric body) and `interface.yaml`
4. Run the automated compliance checker against your skill — it must pass exit 0 (or with only the expected `CAPABILITIES.md` WARN)
5. Open a pull request
6. A reviewer will work through [VETTING.md](./VETTING.md) before merging

Skills that fail any compliance check or critical VETTING item will be rejected.

---

## License + scope

Public, illustrative. See repo settings for license. This is a personal showcase and not a substitute for production skill management.
