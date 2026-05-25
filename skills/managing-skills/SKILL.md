---
name: managing-skills
description: >
  Lifecycle management of installed skills — discovery, evaluation (fit
  + overlap + reputability), the security gate before install,
  installation via API, registry maintenance, usage monitoring, and
  removal. Use when installing, removing, auditing, or recommending
  skills for agents. External discovery + pre-screening belongs to
  `researching-skills`; this skill picks up after a candidate is
  identified.
version: 1.0.0
owner: TANK
status: active
last_modified: 2026-05-24
blast_radius: writes-external
locale_support:
  - en
requires:
  - skill_registry
tags:
  - skills
  - lifecycle
  - governance
  - workflow
---

# managing-skills

Workflow skill for the installed-skill lifecycle: ensures every agent has the right skills, installed safely, at the right scope. Writes-external — updates the skill registry; deletes skills from agents on removal.

## What this skill does

Takes a lifecycle request (install / recommend / audit / search / remove) and: runs the appropriate operation through the security gate, updates the skills registry, verifies the skill works post-install, and produces an installation report or audit summary.

**Boundary clarification:** this skill *manages installed-skill lifecycle*. The discovery of new external candidates belongs to `researching-skills` (SHERLOCK); the actual security-audit logic belongs to `auditing-skill-security` (WATSON). This skill coordinates them.

## When to use it

- Installing or removing a skill on an agent.
- Auditing which skills are installed and whether they are still needed.
- Searching for or recommending skills to fill a capability gap (delegating discovery to `researching-skills`).
- Reviewing the skills registry for conflicts or duplicates.

Do **not** use this skill when:

- The need is pure external discovery — use `researching-skills` directly.
- The need is the security audit itself — use `auditing-skill-security` directly.
- The skill is already audited + approved and only needs installation — operator can install directly via the API; this skill adds value through registry + lifecycle.

## How to use it

1. **Discovery** — monitor capability gaps; accept skill requests from team members. Delegate external scouting to `researching-skills`.
2. **Evaluation** — initial screening: does it fill a real gap? Is there overlap with existing skills? Is it well-documented? Is the source reputable?
3. **Security Gate** — submit to `auditing-skill-security`. Classify as Tier A (internal / curated) or Tier B (community / marketplace). Never install without security approval.
4. **Installation** — install via API. Update skills registry. Verify skill works post-install. Document the installation (who requested, when, why).
5. **Monitoring** — track usage and effectiveness. Flag unused skills for removal. Monitor for conflicts. Schedule periodic re-audits for Tier B skills.
6. **Removal** — remove unused skills (quarterly review minimum). Update registry. Notify affected agents.

## What to escalate

Stop and ask the operator before acting when:

- The skill failed the security gate but business need is urgent — operator decides risk acceptance.
- Installing the skill would create a conflict with an existing installation — operator picks the winner.
- The skill works post-install but produces unexpected behavior — operator confirms before deeper rollout.
- Removing a skill would break a known-active dependency — operator confirms.
- The locale isn't supported (currently `en` only).

## Output format

- **Install** — installation report with verification checklist and registry update confirmation.
- **Recommend** — skill recommendation with rationale and alternatives.
- **Audit** — registry inventory with usage, conflicts, and recommendations.
- **Search** — gap analysis with candidate skills (sourced via `researching-skills`) and fit assessment.
- **Remove** — removal report with affected agents and post-removal verification.

## Constraints

- Never install without security audit approval (via `auditing-skill-security`).
- Never install duplicate functionality — check for overlaps first.
- Never modify a skill after security approval without re-audit.
- Always update the skills registry after any change.
- Monitor context bloat — more skills means more instructions per agent.
- Remove unused skills quarterly — skills should earn their place.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `researching-skills` — external discovery + pre-screening (upstream of this skill's lifecycle)
- `auditing-skill-security` — security gate this skill submits to before any install
