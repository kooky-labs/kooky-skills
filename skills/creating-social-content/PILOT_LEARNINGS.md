# PILOT_LEARNINGS — creating-social-content

**Pilot date:** 2026-05-24
**SPEC version pilot-validated against:** v1.2
**Outcome:** Refactor shipped; SPEC v1.2 holds for workflow-archetype skills with minor framing gaps captured below.

This is the first **workflow-archetype** skill refactored under SPEC v1.2. The connector archetype was previously validated by an earlier pilot; the SPEC was authored primarily with connector skills in mind, so this pilot stress-tested whether workflow skills fit the same contract.

---

## Findings

### F1 — `interface.yaml` schema works for workflow, but "exposes" semantics shift

**What happened:** The SPEC's interface.yaml examples are connector-style — request → external API → response. For a workflow skill, the "exposed capability" is a *prompt surface*, not a network call. The same schema (parameters / returns / capability_flags) worked, but the semantics are different:

- A connector's `parameters` are HTTP body fields. A workflow's `parameters` are prompt inputs.
- A connector's `returns` are API response fields. A workflow's `returns` are the structured output the LLM produces.
- `capability_flags` for connectors describe what the underlying API supports (which currencies, which auth flows, which features). For workflows they describe what the *prompt* supports (which platforms, which pillars, which locales).

**Assessment:** Schema reusable as-is — no shape change needed. But the SPEC prose is connector-centric and would mislead a workflow author. **Suggested SPEC amendment:** add a "Workflow archetype" subsection with a concrete workflow example showing how to interpret the same fields.

### F2 — `blast_radius` semantics ambiguous for content-generation skills

**What happened:** This skill takes a topic and returns post copy. Internally, the skill performs no external writes — it's a pure prompt. But the *output* is intended for publication to social media (writes-external in spirit).

**Decision made:** `blast_radius: read-only`. Reasoning: blast_radius describes what the **skill** does, not what the **caller** does with the output. The skill outputs text to its caller; the caller chooses to publish. The same logic applies to any read-only data-fetching skill: returning data is read-only even when the caller uses that data to trigger writes downstream.

**Alternative interpretation considered:** `writes-external` because the output is a publishing payload that almost always gets shipped to a public platform.

**Assessment:** The current SPEC enum lets the author pick either reasonably. **Suggested SPEC amendment:** Add a one-liner clarifying that `blast_radius` describes the skill's own actions, not what callers do with outputs. Otherwise this gets re-litigated for every content-generation skill.

### F3 — Pure-prompt skills + `requires: []`

**What happened:** The skill needs no external tools, no API keys, no env capabilities. It's pure LLM. So `requires: []`.

**Open question:** Should pure-prompt skills declare a `claude-llm` or `inference` capability explicitly? Or is LLM access an implicit baseline that doesn't need declaration?

**Current interpretation:** Implicit baseline. The connector example skills declare things like `network-egress` and secret-store integrations — those are non-baseline capabilities. LLM access is the runtime substrate every skill runs on; declaring it would be like declaring "bash" or "filesystem-read".

**Assessment:** Implicit baseline is the right call. **Suggested SPEC amendment:** Add a sentence under `requires:` saying "Baseline runtime capabilities (LLM inference, stdin/stdout) are implicit and do not need to be declared."

### F4 — `i18n/` skip rule is clear, but worth restating

**What happened:** This skill generates social posts in English. Its *output* is user-facing copy, but the SKILL.md body is operator-facing (drives the LLM). Per the SPEC, `i18n/` is "mandatory if skill has any user-facing copy" — does generated output count?

**Decision made:** No `i18n/` directory. The skill itself emits no user-facing copy from its own surface (no status messages, no receipts, no errors shown to humans). Generated content is deliverable output, not skill UI. `locale_support: [en]` controls *what* the skill can produce, not whether the skill needs i18n bundles.

**Assessment:** SPEC is correct as written but the distinction takes a second pass to internalize. **Suggested SPEC amendment:** Add a clarifying note: "User-facing copy means strings shown to humans *by the skill itself* (status updates, error messages, receipts) — not strings the skill *generates as deliverable content* (drafted posts, written summaries, translated documents)."

### F5 — Body sections map cleanly to workflows

**What happened:** Restructuring the body to **What this skill does** / **When to use it** / **How to use it** / **What to escalate** worked well. The existing body had Process/Output/Constraints — close to How/Output/Anti-patterns but not exactly. Mapping took one pass.

The "What to escalate" section was the most useful addition — previously the skill had no guidance for edge cases (defamatory content, unverifiable claims, locale mismatches). Forcing the section made the skill safer.

**Assessment:** The four-section operator-centric convention generalizes correctly across archetypes. No change needed.

### F6 — Cluster overlap flagged for future cleanup

**What happened:** While auditing, noticed potential overlap among `creating-social-content`, `copywriting`, and `planning-content`. Not in scope for this pilot.

**Action:** Surface for the next bulk-refactor round. A cluster review should run before bulk SPEC v1.2 work on this set.

### F7 — Stale duplicate cleanup

**What happened:** A stale `social-content/` skill existed in this repo — an incomplete stub (intake questionnaire only, no actual drafting logic, plus a duplicate-frontmatter bug). Deleted as part of this pilot. No unique content lost.

---

## Compliance gate result

```
Item  6: PASS  blast_radius declared honestly
Item  7: PASS  requires declares env capabilities
Item  8: PASS  body covers what/when/how/escalate
Item  9: PASS  no hardcoded user-facing strings
Item 10: WARN  no conflicting capability claims (capability registry absent — expected)
Item 11: PASS  connector scripts use error-handling pattern (n/a — no scripts)

RESULT: compliant with warnings (exit 0)
```

---

## Suggested SPEC amendments (consolidated)

These are candidates for the next SPEC revision (or the deferred-amendments register). Each has a trigger condition; revisit when met.

| # | Amendment | Trigger to codify |
|---|---|---|
| W1 | Add a "Workflow archetype" subsection to interface.yaml schema docs with a content-drafting example | When a second workflow skill ships and confirms the schema works |
| W2 | Clarify `blast_radius` is about the skill's actions, not caller use of output | When a third content/generation skill picks a different value than this one |
| W3 | Add note: baseline runtime (LLM, stdin/stdout) is implicit for `requires:` | Next pure-prompt workflow skill — codify if same decision repeats |
| W4 | Clarify the `i18n/` rule: user-facing = skill's own surface, not deliverable content | When a translation or summarization skill faces the same question |

None of these block subsequent refactors. All are clarifications, not behavioral changes.

---

## Readiness signal

The pilot validates that:

- ✅ The 9-field frontmatter applies cleanly to workflow skills
- ✅ `interface.yaml` schema reusable; only documentation needs workflow examples
- ✅ The four-section operator-centric body generalizes across archetypes
- ✅ The compliance checker correctly grades workflow skills
- ⚠️ Cluster overlap exists in this repo (per F6) — address before bulk-refactor work
