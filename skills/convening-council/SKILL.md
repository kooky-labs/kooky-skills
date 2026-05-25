---
name: convening-council
description: >
  Convene a multi-agent council for high-stakes decisions. Gathers parallel
  recommendations from 2-4 agents, runs an adversarial counter-argument pass,
  synthesizes one auditable decision, and writes it to the `decisions` table.
  Use for architectural, security, strategic, or multi-domain calls — not for
  simple or time-sensitive tasks. Protocol, not infrastructure: depends on
  callable_agents + decisions/plan_actions tables existing at runtime.
version: 1.0.0
owner: KAI
status: active
last_modified: 2026-05-24
blast_radius: writes-external
locale_support:
  - en
requires:
  - callable_agents
  - decisions_table
  - plan_actions_table
tags:
  - governance
  - decision-making
  - multi-agent
  - workflow
---

# convening-council

Workflow protocol skill for high-stakes decisions where a single agent's answer is not enough. Writes one auditable row to the `decisions` table.

## What this skill does

Takes a high-stakes question, runs it in parallel through 2-4 agents, finds the strongest recommendation, then runs an adversarial counter-argument pass against it to surface failure modes that consensus would miss. Synthesizes the result into a single `decisions` row with the strongest objection preserved verbatim.

This skill is a **protocol, not infrastructure.** It uses existing runtime capabilities (`callable_agents` for parallel agent invocation; `decisions` and `plan_actions` tables for persistence) — declared in `requires:` above. If those don't exist at runtime, the skill cannot execute.

## When to use it

Use council for questions with real downside if wrong:

| Category | Trigger examples |
|---|---|
| `architectural` | Two or more valid technical approaches; hard to reverse |
| `security` | Installing a non-verified skill; exposing a new endpoint; auth change |
| `strategic` | Pricing change, positioning shift, hire/fire, partnership commitment |
| `multi_domain` | Requires cross-functional judgment (marketing + sales + technical) |

Do **not** use this skill when:

- The task has one correct answer — council adds latency without signal.
- The operation is time-sensitive — council takes longer.
- The work is implementation — one agent should build, not a panel.
- The user's preference is the answer — escalate to approval, not council.

## How to use it

### Step 1 — Frame the question

Write it in one sentence. No leading language. Include context agents need but not your preferred answer.

- Bad: *"Shouldn't we just use Stripe since it's obviously better?"*
- Good: *"Select payment processor for KOOKY OS client billing. Requirements: subscriptions, metered usage, pause/resume, LATAM card support. Budget for platform fees < 3.5%."*

### Step 2 — Pick 2-4 agents

Include domain owners plus at least one outsider perspective. Fewer than 2 defeats the purpose. More than 4 produces mush.

| Question type | Panel composition |
|---|---|
| Architectural | JARVIS + WATSON + one domain owner |
| Security | WATSON + JARVIS + TANK |
| Strategic | KAI + HOUDINI + MAVERICK |
| Skill install | WATSON + TANK + SHERLOCK |
| Pricing / GTM | MAVERICK + HOUDINI + FINN |

### Step 3 — Parallel proposal pass

Use `callable_agents` to ask each panelist the same question in parallel. Give each:

- The question verbatim
- The same context
- This instruction: *"Give your recommendation and the reasoning behind it. Be specific. Do not hedge."*

Log each response to `plan_actions` with `category='council_response'` as it arrives.

### Step 4 — Adversarial counter pass

After responses arrive, pick the strongest recommendation. Route it to a designated red-team agent (default: WATSON; for WATSON's own recommendation use JARVIS) with this instruction:

> *"Find the strongest counter-argument to this recommendation. Not a hedge — a specific failure mode, missed assumption, or scenario where this is clearly wrong. If you cannot find one, say so."*

This is where real signal appears. Consensus without dissent is groupthink.

### Step 5 — Rebuttal (optional, one round only)

If the counter-argument is substantive, give the original agent one chance to rebut or revise. One round, no loops.

### Step 6 — Synthesize and decide

Write the `decisions` row. Required fields:

| Field | Content |
|---|---|
| `title` | Short name for the decision |
| `category` | From the table in Step 1 |
| `decision` | The chosen path, one paragraph |
| `rationale` | Why this over alternatives |
| `alternatives` | JSON array: each option considered + why rejected |
| `strongest_objection` | The adversarial counter-argument, verbatim |
| `evidence` | JSON array of `{session_id, message_range, note}` pointers |
| `decided_by` | `council:<comma-separated agent codes>` |
| `protocol` | `council` or `adversarial` |
| `routes_to_approval` | `true` if human sign-off needed |

### Step 7 — Route

- Low-impact + high-agreement → implement directly
- High-impact OR unresolved dissent → set `routes_to_approval=true`; appears in the Approvals queue for operator review

## What to escalate

Stop and ask the operator before convening when:

- The question isn't framed cleanly — re-frame first; bad framing produces bad council output.
- The required runtime capabilities aren't present (`callable_agents`, `decisions`, `plan_actions`) — abort rather than partial-execute.
- The panel composition would have a domain conflict-of-interest — operator picks the panel.
- The decision crosses the operator's preference territory (not an agent capability question) — escalate.
- The locale isn't supported (currently `en` only).

## Output

One row in `decisions`. Full deliberation transcript accessible via `evidence` pointers (session_id + message_range) which deep-link into the originating session view. No separate council storage.

## Constraints

- One council row per decision. Do not open a second council on the same question without new information.
- `strongest_objection` must be populated. An empty objection means the counter-pass was skipped — not acceptable.
- Never invite more than 4 agents. Output quality degrades fast above 4.
- Council does not replace the operator. For reversible decisions inside an agent's domain, the agent decides. For high-stakes + strategic, escalate.
- Do not retry a council. If the first pass was bad, diagnose why; don't rerun hoping for a different answer.

## Anti-patterns

- **Consensus theater.** Asking agents to "vote" or rate confidence produces noise. Ask for reasoning, not scores.
- **Stacking the panel.** Inviting only agents likely to agree defeats the purpose. Include at least one outsider.
- **Running council for user preferences.** "What color should the button be?" is not a council question. Ask the operator.
- **Skipping the adversarial pass.** Proposal-only councils are just averaged opinions.
- **Using council to avoid accountability.** One agent should still own the decision. Council informs; it does not dilute responsibility.

## References

- `interface.yaml` — machine-readable capability surface
- `PILOT_LEARNINGS.md` — workflow-archetype findings surfaced during SPEC v1.2 refactor (notably W5 on runtime-infrastructure `requires:`)
- SPEC v1.2 §2.3 — operator-centric body section convention
- `delegating-tasks` — for simple routing; council is the heavier protocol for high-stakes decisions
