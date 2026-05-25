---
name: conducting-outreach
description: >
  Crafts personalized cold outreach messages for LinkedIn and email,
  including multi-touch follow-up sequences. Use when writing sales
  outreach to a specific prospect — the goal is to start a conversation,
  not close a sale. Consumes `qualifying-leads` output (tier +
  prospect_data + key_observations) when available; produces messages
  ready to send. Does not send.
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
  - outreach
  - workflow
---

# conducting-outreach

Workflow skill for drafting personalized, value-driven outreach that starts real conversations. Read-only — produces ready-to-send messages; never sends them.

## What this skill does

Takes prospect details (ideally from a `qualifying-leads` report) plus the desired message format and returns a personalized message, character/word count, personalization notes (what was customized and why), suggested follow-up timing, and alternative subject lines (for email). The skill applies a format-specific framework to every draft and enforces a strict "no pushy / no urgency tactics / no fake scarcity" rule.

When called with `qualifying-leads` output as input, the skill uses the `qualification_tier` to set message intensity (Hot = direct ask + specific time slot; Warm = lighter touch + open-ended question; Cool = content offer + no ask), the `prospect_data` for personalization, and the `key_observations` for opening lines.

## When to use it

- Writing a first-touch message to a prospect (LinkedIn or email).
- Drafting follow-up messages in a sequence (follow-up 1, follow-up 2, breakup).
- Personalizing outreach based on a prospect's recent activity.
- Refreshing a stale outreach draft after new prospect information emerges.

Do **not** use this skill when:

- The prospect isn't qualified yet — use `qualifying-leads` first.
- The prospect is an existing customer — different workflow (use a customer-success skill).
- The need is mass-broadcast — this skill is one-to-one outreach by design.
- The locale isn't supported (currently `en` only).

## How to use it

1. **Gather prospect details:** name, title, company, situation, pain signals, ICP match reason, and — if available — the `qualifying-leads` output (tier + prospect_data + key_observations).
2. **Select the format and apply its framework:**
   - **LinkedIn connection request** (300 chars max) — Reference something specific. State reason in one sentence. No pitch, links, or ask.
   - **LinkedIn DM** (first touch, under 150 words) — Open with an observation about them. Connect to value prop naturally. End with one question. No attachments or links.
   - **Cold email** (first touch, under 125 words) — Subject under 50 chars. Personalized first line. Value prop in 1-2 sentences. Social proof. Low-commitment CTA. Plain text.
   - **Follow-up 1** (3-4 days later) — New angle, new value. Never "just checking in."
   - **Follow-up 2** (5-7 days later) — Share something relevant. No pressure.
   - **Breakup** (7-10 days later) — Honest close, no hard feelings.
3. **Adjust intensity by qualification tier** (when known):
   - **Hot** — direct ask, specific time slot, brief
   - **Warm** — lighter touch, open-ended question
   - **Cool** — content offer (article / resource), no ask
   - **Cold** — usually skip outreach entirely; if necessary, drop into a nurture sequence
4. **Personalize every message** to the specific prospect. Generic templates fail this skill's contract.
5. **Apply voice:** conversational, direct, confident, never pushy.
6. **Return the output** in the format below.

## What to escalate

Stop and ask the operator before drafting when:

- The prospect data hasn't been qualified — qualification shapes intensity; un-qualified outreach is often wasted.
- The prospect appears to be a competitor, partner, or existing customer (mis-routed).
- The brief asks for urgency tactics, fake scarcity, or financial promises — these violate constraints below; confirm before proceeding.
- The prospect has previously asked not to be contacted (per any data available) — surface, do not draft.
- The locale isn't supported (currently `en` only).

## Output format

- Message ready to send
- Character / word count
- Personalization notes (what was customized and why — 2-4 bullets)
- Suggested follow-up timing (when to send the next message in the sequence)
- Alternative subject lines (for cold email only — 2-3 variants)

## Constraints

- Never send the same message to multiple prospects (this skill produces one-to-one only).
- Never promise specific financial outcomes.
- Never disparage the prospect's current situation or vendors.
- Never use urgency tactics ("limited spots", "price going up").
- Never attach files or include links in first-touch LinkedIn DMs.
- Never follow up more than 3 times without a response (breakup ends the sequence).
- Never use "I" as the first word of the message.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
- `qualifying-leads` — upstream input source (qualification_tier + prospect_data + key_observations)
