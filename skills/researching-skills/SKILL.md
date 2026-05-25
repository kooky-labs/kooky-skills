---
name: researching-skills
description: >
  Discovers candidate skills externally (community registries, GitHub,
  global installations) and pre-screens them for fit, compatibility, and
  security red flags. Use when searching for existing skills to fill a
  capability gap before building from scratch. Pre-installation discovery
  only — installation lifecycle belongs to `managing-skills`.
version: 1.0.0
owner: SHERLOCK
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - research
  - skills
  - discovery
  - workflow
---

# researching-skills

Workflow skill for finding existing skills from external sources rather than building from scratch. Read-only — discovers + pre-screens candidates; never installs.

## What this skill does

Takes a capability gap and returns: ranked candidate skills from external sources (with name, source, install count, description), per-candidate fit + compatibility assessment, security red flags requiring review, and a recommendation per candidate (adopt / adapt / skip with reasoning).

**Boundary clarification:** this skill *discovers and pre-screens*. The full security audit + installation + monitoring lifecycle belongs to `managing-skills` (TANK). The two skills do different work even though both have an "evaluation" step.

## When to use it

- An agent needs a new capability and an existing skill might already cover it.
- Scouting registries and marketplaces for useful skills.
- Evaluating whether a community skill is plausibly worth bringing in.

Do **not** use this skill when:

- The skill is already installed and needs lifecycle management — use `managing-skills`.
- The need is security audit of a specific skill — use `auditing-skill-security`.
- The capability isn't well-defined yet — define the gap first.

## Discovery sources

- **skills.sh** — community registry with install counts and ratings
- **GitHub search** — search for SKILL.md files and agent-skill repositories
- **Globally-installed skills** (`~/.claude/skills/`) — battle-tested skills already in production use locally

## How to use it

1. **Scope the search** — what capability is needed, which agent(s) would use it.
2. **Search all sources** by keyword and capability match.
3. **Evaluate each candidate:**
   - Does it match the needed capability?
   - Is it compatible with the target platform (no local-only dependencies that won't transfer)?
   - Trust level — Official > Curated (>10K installs) > Community (>1K) > Experimental (<1K).
   - Code quality and documentation.
   - Security red flags — `eval`, shell exec, network calls to unknown domains.
4. **Rank candidates by fit.** Flag any needing deep security review.
5. **Recommend per candidate** — adopt as-is / adapt for context / skip — with reasoning for each.

## What to escalate

Stop and ask the operator before issuing the recommendation when:

- Multiple equally-good candidates exist — operator preference is the tie-breaker.
- A high-fit candidate has security flags that would require operator-level risk acceptance.
- No skills match — surface "no matches" with alternative search-term suggestions; don't force a recommendation.
- The capability gap suggests the skill should be authored rather than adopted.
- The locale isn't supported (currently `en` only).

## Output format

- Skills found — name, source, install count, description, link
- Per-candidate relevance + compatibility assessment
- Security flags for review
- Recommendation per skill (adopt / adapt / skip) with reasoning

## Constraints

- Never install directly — discovery + pre-screening only. All adoption decisions go through `managing-skills` + `auditing-skill-security`.
- Every recommendation must include reasoning, not just "looks good."
- Include the skill's source URL so security can audit it.
- If no skills match, report "no matches" with alternative search-term suggestions.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `managing-skills` — lifecycle management of installed skills (boundary partner to this skill)
- `auditing-skill-security` — runtime security audit after this skill identifies a candidate
