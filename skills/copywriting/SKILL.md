---
name: copywriting
description: >
  Writes clear, compelling marketing copy — headlines, body, CTAs, email
  subject lines, ad copy, product descriptions, and taglines. Use when
  drafting or improving copy for landing pages, emails, ads, or product
  descriptions. For social media posts use `creating-social-content`
  instead; for content scheduling use `planning-content`.
version: 1.0.0
owner: HOUDINI
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - copywriting
  - marketing
  - workflow
---

# copywriting

Workflow skill for drafting and rewriting marketing copy fragments that ship to landing pages, emails, ads, and product pages. Read-only — returns finished copy; never publishes.

## What this skill does

Takes a copy type, its destination context, the target audience, and the desired user action — returns finished copy plus A/B variants where they help (headlines, subject lines, CTAs). The skill enforces brand voice and a banned-word list on every draft, and runs three quality checks before returning output: read-aloud test, specificity test, "so what?" test.

This skill drafts copy *fragments* (headlines, paragraphs, CTAs, subject lines, taglines). For full social posts use `creating-social-content`; for long-form (articles, newsletters, blogs) use a long-form content skill.

## When to use it

- Writing or rewriting copy for a landing page, email, ad, or product page.
- Generating A/B headline or subject-line variants for testing.
- Crafting CTAs or taglines for a specific audience segment.
- Improving existing copy that's underperforming or stale.

Do **not** use this skill when:

- The target is a social media post — use `creating-social-content` (different platform constraints, hooks, length budgets).
- The target is long-form (newsletter, blog, article) — wrong scope.
- The need is a content calendar / schedule — use `planning-content`.
- The brief asks for paid-ad copy with regulated claims — escalate to operator review first; this skill applies brand-voice rules but cannot verify compliance.

## How to use it

1. **Identify inputs:** copy type, placement context (where it ships), target audience, desired action.
2. **Apply the matching framework:**
   - **Headlines** — lead with outcome, use specific numbers, pattern: [Pain point] + [Unexpected solution].
   - **Body copy** — open with empathy, one idea per paragraph, features become benefits, single clear CTA.
   - **CTAs** — action verbs ("Start building", "Get your AI team"), match commitment level to funnel stage.
   - **Email subject lines** — under 50 characters, curiosity gap or specific benefit, produce 2-3 variants.
   - **Email body** — one purpose per email, first sentence earns the second, PS for secondary CTA.
   - **Product descriptions** — what it does → who it's for → why it's different, in that order.
   - **Taglines** — under 7 words, no commas, says one thing and means it.
3. **Apply brand voice:** direct, specific, honest, aspirational without hype. Concrete numbers and names beat abstractions.
4. **Run quality checks:**
   - Read-aloud test — does it sound like a human said it?
   - Specificity test — could another company ship this exact copy without changing a word? If yes, sharpen.
   - "So what?" test — does the reader understand why they should care?
5. **Generate A/B variants** for headlines and subject lines (2-3 each). Skip variants for body copy unless asked.
6. **Return the output** in the format below.

## What to escalate

Stop and ask the operator before drafting when:

- The copy makes claims about results (revenue, conversion lift, growth) the operator hasn't confirmed are public — never fabricate numbers, even small ones.
- The copy names a real person, partner, or competitor in a way that could be defamatory or misrepresent them.
- The brief asks for fear-based copy ("you'll be left behind") or financial / health / income promises — these violate constraints below; confirm before proceeding.
- The locale isn't supported (currently `en` only).

## Output format

- Final copy, formatted for its destination
- Character / word count where relevant (headlines, subject lines, ad copy)
- 2-3 A/B variants for headlines and subject lines
- Brief rationale for key creative choices (one line each)

## Constraints

- Banned words: leverage, synergy, disrupt, paradigm, revolutionary, game-changing, unlock.
- Never make unverifiable claims about results.
- Never use fear-based copy ("You'll be left behind").
- Never promise specific income or financial outcomes.
- Never disparage competitors by name.
- Limit to 0–1 emoji per copy unit; zero is preferred outside of subject lines.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
