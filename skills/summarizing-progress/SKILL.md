---
name: summarizing-progress
description: >
  Generates concise executive summaries and progress reports — daily
  briefings, weekly summaries, and project status updates. Use when
  stakeholders need a clear picture of what happened, what is blocked,
  and what needs attention. Reports on work other agents/skills produced;
  does not produce the underlying work itself.
version: 1.0.0
owner: KAI
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - reporting
  - summary
  - workflow
---

# summarizing-progress

Workflow skill for executive-level status reports. Read-only — generates the report; never modifies the underlying activity data.

## What this skill does

Takes a report type, a time window, and activity data — returns a structured report tuned to the type (daily / weekly / project status). The skill leads with outcomes (not activities), quantifies wherever possible, and always surfaces blockers and decisions-needed with enough context to act on them.

This skill reports on work other skills and agents produced. It doesn't gather the raw data itself — the caller passes activity data in, or points the skill at a source.

## When to use it

- Generating a daily briefing or weekly summary for the operator.
- Producing a project status update for a stakeholder.
- Summarizing agent activity for cross-functional visibility.
- Closing a sprint / cycle / milestone with a wrap-up document.

Do **not** use this skill when:

- The report needs implementation detail — wrong scope; this stays executive-level.
- The audience is one person who already has the context — a one-line update is enough.
- The data isn't gathered yet — gather first; this skill won't fabricate.
- The need is forward-looking strategy, not progress — use a planning skill instead.

## How to use it

1. **Identify the report type and time window** (daily / weekly / project status; today / this week / sprint / quarter).
2. **Receive activity data, open items, and blockers** from the caller. If data is missing for a department or area, flag it — never guess.
3. **Generate the appropriate format:**
   - **Daily Briefing** (under 200 words) — Done Today / In Progress / Blocked / Needs Attention / Tomorrow's Priority.
   - **Weekly Summary** (under 500 words) — Top 3 wins / Department breakdown with metrics / Blockers & Risks / Next Week's Priorities / Decisions Needed.
   - **Project Status** — Status (On Track / At Risk / Blocked) / progress % / completed milestones / current work / upcoming / risks / dependencies.
4. **Lead with outcomes, not activities.** "Closed 2 leads" beats "Sent 15 emails."
5. **Quantify wherever possible** — numbers over adjectives.
6. **End with clear next actions and decisions needed** with enough context to act on them without a follow-up.

## What to escalate

Stop and ask the operator before issuing the report when:

- Activity data is missing for a meaningful department — fabricating would mislead.
- A blocker is severe enough that surfacing it first changes the report's shape — confirm before downplaying.
- The audience isn't named — different audiences need different framings.
- The locale isn't supported (currently `en` only).

## Output format

Structured report matching the requested type. Every report includes:

- Attribution of work to the agent or person who did it
- Blockers with enough context to unblock
- Decisions needed with enough context to decide

## Constraints

- Never fabricate progress or metrics.
- Never hide bad news or pad reports with filler.
- If data is missing for a department, say so rather than guessing.
- Keep it executive-level — no implementation details.
- Numbers beat adjectives. Always.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `delegating-tasks` — pairs with this skill (what got delegated to whom, then what they produced)
