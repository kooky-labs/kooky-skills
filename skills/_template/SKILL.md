---
name: <skill-name>
description: >
  <One paragraph — what the skill does + when to use it. First line is the
  index entry that surfaces in skill catalogs.>
version: 1.0.0
owner: <agent-name-or-Renato>
status: draft
last_modified: <YYYY-MM-DD>
blast_radius: read-only
locale_support:
  - en
requires: []
# Optional fields per SPEC §2.2:
# tags: [tag1, tag2]
# deprecated_in: 2.0.0       # if status: deprecated
# superseded_by: other-skill # if status: superseded
---

# <skill-name>

Brief one-sentence framing of the skill — what it is, who it's for.

## What this skill does

Concrete description of what the skill outputs and what hard constraints it enforces. Two short paragraphs max.

## When to use it

- Trigger condition 1
- Trigger condition 2
- Trigger condition 3

Do **not** use this skill when:

- Anti-pattern 1
- Anti-pattern 2

## How to use it

Step-by-step procedure with judgment markers. The body answers "how does an operator drive this skill?" Not Python pseudocode — actual operator guidance.

1. **First step** — what to do, what to check.
2. **Second step** — branch on conditions if relevant.
3. **Output** — what gets returned to the caller.

## What to escalate

Stop and ask the operator before proceeding when:

- Edge case 1 (what makes it edgy + what to ask)
- Edge case 2
- Anything that requires judgment outside the skill's mandate

## References

- `interface.yaml` — machine-readable capability surface
- `PILOT_LEARNINGS.md` — created on first non-trivial use; captures gaps surfaced against the SPEC

---

**Scaffold instructions** (delete this section once the skill is filled in):

1. Copy this directory to `skills/<your-skill-name>/`.
2. Fill in the 9 required frontmatter fields above.
3. Author `interface.yaml` (use `_template/interface.yaml` as starter — defines `exposes`, `consumes`, `capability_flags`).
4. Rewrite the four body sections (`What this skill does` / `When to use it` / `How to use it` / `What to escalate`) for your skill.
5. If the skill emits user-facing copy (status messages, receipts, error text shown to humans), add `i18n/en.json` (and `i18n/<locale>.json` for each additional locale the skill supports).
6. Run the compliance checker against your skill — must return exit 0 (PASS or known WARNs only).
7. Optionally `references/` (exemplars), `assets/` (raw materials), `scripts/` (frozen helper code) — see SPEC §1.
8. Delete this "Scaffold instructions" section once your skill is complete.

Full SPEC: `_OS/templates/skill-template/SPEC.md` (or the linked copy in the repo root).
