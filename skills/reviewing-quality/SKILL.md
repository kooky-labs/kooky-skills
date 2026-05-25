---
name: reviewing-quality
description: >
  Reviews deliverables — content, code, agent outputs, and business materials —
  against quality, accuracy, and brand standards. Use before any deliverable
  reaches a client or goes to production. Returns a verdict (PASS / PASS WITH
  CHANGES / FAIL) plus a severity-classified findings table and at least one
  strength.
version: 1.0.0
owner: WATSON
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - quality
  - review
  - workflow
---

# reviewing-quality

Workflow skill for operator-side quality review of any deliverable before it ships. Read-only audit — never rewrites the work; returns specific fix instructions.

## What this skill does

Takes a deliverable plus its context and returns a structured review: a verdict, a findings table classified by severity, at least one identified strength, and a clear next-step recommendation. The skill applies five review dimensions consistently across deliverable types so reviews from different sessions are comparable.

This is the human-judgment quality pass. It does not replace automated linting / type-checking / test runs — those happen upstream. Use this skill when a human-equivalent review is the gate before publishing or shipping.

## When to use it

- Reviewing content before publishing (social posts, emails, copy, articles).
- Reviewing code before deployment (PRs, hotfixes, infrastructure changes).
- Reviewing agent outputs before they reach a client or customer.
- Reviewing business materials (proposals, reports, decks, contracts).

Do **not** use this skill when:

- Pure format / lint / type-check is what's needed — use the appropriate automated tool instead.
- Drafting or rewriting the deliverable — this skill reviews; it does not author.
- Approving stylistic preferences — block only on substantive issues.

## How to use it

1. **Receive inputs:** the deliverable, its type, the author (optional), the intended audience, and what the deliverable is for (the context).
2. **Review across five dimensions:**
   - **Factual Accuracy** — Are claims verifiable? Any fabricated details? Sources cited where needed?
   - **Brand Voice** — Sounds authentic? Banned words present? (leverage, synergy, disrupt, paradigm, revolutionary, game-changing)
   - **Technical Accuracy** (code only) — Works as intended? Security vulnerabilities? Edge cases handled?
   - **Bias and Safety** — Unintentional bias? Misleading statements? Promises that cannot be kept?
   - **Formatting** — Platform-appropriate? Spelling and grammar? Clear structure?
3. **Classify each finding by severity:**
   - **BLOCK** — must fix before publishing or deploying (factual errors, security issues, brand damage).
   - **MAJOR** — should fix (significant quality issues that aren't catastrophic).
   - **MINOR** — nice to fix (polish items; not blockers).
   - **NOTE** — observation only (no action required).
4. **Identify at least one strength.** Reviews that only surface problems give a distorted signal.
5. **Issue the verdict:**
   - **PASS** — ship as-is.
   - **PASS WITH CHANGES** — ship after MINOR/MAJOR fixes; no re-review required.
   - **FAIL** — do not ship until BLOCK findings are addressed; full re-review required.
6. **Return the output** in the format below, plus a clear next-step recommendation.

## What to escalate

Stop and ask the operator before issuing a verdict when:

- The deliverable touches legal, medical, or financial claims that need domain-expert review beyond this skill's scope.
- The factual claim is verifiable but verification would require external research the operator hasn't authorized.
- A BLOCK finding might be a stylistic disagreement rather than a substantive issue — flag the doubt rather than blocking unilaterally.
- The deliverable is in a locale this skill doesn't support (currently `en` only).
- The deliverable type doesn't match any of the four supported categories (content / code / agent output / business material).

## Output format

```
VERDICT: PASS / PASS WITH CHANGES / FAIL
Rationale: [1-2 sentences]

Findings:
  - [SEVERITY] [Dimension] Description → Fix recommendation
  - ...

Strengths:
  - [at least one]

Next step: [specific recommendation]
```

## Constraints

- Never approve with unresolved doubts — flag the doubt explicitly.
- Never rewrite the deliverable — provide specific fix instructions instead.
- Never block for stylistic preferences — only for substantive issues.
- Be specific: "This claim is unverified" not "Some claims may need checking."
- Apply all five dimensions even when one seems irrelevant — explicitly mark as N/A rather than skipping silently.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
