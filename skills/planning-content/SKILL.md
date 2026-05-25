---
name: planning-content
description: >
  Plans content calendars, selects topics, and aligns publishing schedules
  with business objectives. Use when deciding what content to create, when
  to publish, or how to allocate effort across platforms. Plans content;
  does not draft. Use `copywriting` or `creating-social-content` for
  drafting itself.
version: 1.0.0
owner: HOUDINI
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - content
  - planning
  - calendar
  - workflow
---

# planning-content

Workflow skill for planning what content to create, when to publish, and how to allocate effort across platforms. Read-only — returns calendars and strategy; never writes the content itself.

## What this skill does

Takes a time horizon (week / month / quarter), a business objective, and the active platforms — returns a structured plan: weekly grid, monthly calendar, or quarterly strategy. The plan allocates content across the four founder-voice pillars using the current campaign weighting, maps a default weekly rhythm, and reserves capacity for reactive/trending content.

The skill **plans** content but never **drafts** it. Use `copywriting` for marketing-copy fragments and `creating-social-content` for platform-native social posts.

## When to use it

- Planning a weekly, monthly, or quarterly content calendar.
- Deciding which topics or themes to prioritize given a business goal.
- Allocating effort across platforms (X, LinkedIn, TikTok, Instagram).
- Aligning content output with a product launch, campaign, or growth target.

Do **not** use this skill when:

- The need is drafting copy or posts — use `copywriting` or `creating-social-content`.
- The horizon is a single post on a single platform with no calendar context — overkill; use the drafting skills directly.
- The output is paid-ad media planning (placement, spend, targeting) — different domain; out of scope.

## How to use it

1. **Clarify inputs:** time horizon (week / month / quarter), business objective (audience growth / lead-gen / brand awareness / product launch), active platforms with current posting cadence.
2. **Allocate across the four founder-voice pillars** using the current 2026-Q2 mix:
   - **Case Studies & Proof — 35%** (client wins, measurable outcomes, before/after, shipped artifacts)
   - **Industry Hot Takes — 30%** (opinions on AI tooling trends, LATAM market dynamics, partner-ecosystem patterns, agentic-era SaaS)
   - **Founder's Journey — 20%** (the build-in-public story, decisions, lessons, milestones)
   - **Behind-the-Scenes Ops — 15%** (how the company runs, agent stack, daily reality)

   > **Pillar mix note:** values current as of 2026-Q2. Scheduled revisit after 2026-06-26. Previous mix: 40/25/20/15 (Founder's / BTS / Case Studies / Hot Takes).

3. **Map a default weekly rhythm:** Monday (case study), Tuesday (hot take), Wednesday (case study), Thursday (hot take), Friday (founder's journey or BTS). Adjust to actual material.
4. **Plan repurposing** — one strong piece becomes 3-4 platform-native posts (each tuned to its platform, not copy-paste).
5. **Reserve ~20% capacity for reactive/trending content** — leave room to respond to surprises.
6. **Set measurable targets** per platform and timeframe (volume, engagement, top-of-funnel adds — whichever maps to the objective).
7. **Return the output** in the format below.

## What to escalate

Stop and ask the operator before issuing the plan when:

- The business objective isn't clear or has multiple competing goals — the pillar allocation depends on the objective.
- The active-platform list is missing or inconsistent with the operator's actual publishing surfaces.
- The requested cadence exceeds documented production capacity — over-committing produces filler.
- The plan would require fabricating experiences or numbers — never plan content that needs invented material.
- The locale isn't supported (currently `en` only).

## Output format

- **Weekly plan** — table with columns: Day / Platform / Pillar / Topic / Format / Notes.
- **Monthly calendar** — week-by-week themes, key pieces, production schedule, metric targets.
- **Quarterly strategy** — narrative arc, platform evolution, pillar adjustments, resource needs.

## Constraints

- Never plan content that requires fabricating experiences.
- Quality over volume — fewer posts with impact beats daily filler.
- The calendar is a guide, not a mandate. Skip anything that isn't genuinely interesting.
- Always account for available production capacity before committing to volume.
- The pillar percentages are policy, not law — operator can adjust per campaign; document the adjustment in the plan if it deviates from the documented mix.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `creating-social-content` — drafts the social posts this calendar schedules
- `copywriting` — drafts the marketing copy fragments this calendar schedules
