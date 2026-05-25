# PILOT_LEARNINGS — convening-council (SPEC v1.2 refactor)

Captured during the bulk SPEC v1.2 refactor pass. Only includes findings that surface new SPEC gaps; routine conversions do not produce PILOT_LEARNINGS files.

This file extends the running W-amendment list started by the workflow-archetype pilot — see `creating-social-content/PILOT_LEARNINGS.md` for W1-W4. This pass adds **W5**.

---

## W5 — Runtime-infrastructure dependencies in `requires:`

### Finding

SPEC v1.2 examples for `requires:` are either empty (`requires: []` for pure-prompt workflow skills) or implicit (LLM + stdin/stdout baseline). `convening-council` is the first refactored skill that genuinely depends on runtime capabilities beyond the LLM baseline:

- `callable_agents` — the ability to invoke other agents in parallel
- `decisions_table` — persistent storage for the decision row
- `plan_actions_table` — append-only log for `council_response` events

The skill cannot execute without these. The current SPEC §2.1 `requires:` description doesn't codify how to declare runtime infrastructure (vs. external libraries, vs. environment variables, vs. data sources).

### What was done in this refactor

Adopted a simple list format pending a SPEC amendment:

```yaml
requires:
  - callable_agents
  - decisions_table
  - plan_actions_table
```

The compliance gate accepted this shape. Each entry is also surfaced in `interface.yaml` under `consumes:` as `type: capability` so a downstream consumer can see the dependency from either side.

### Proposed amendment (W5)

Add to SPEC §2.1 (or a new §2.1.1) a short section codifying the conventions for `requires:`:

1. **Empty list** — skill depends only on the LLM baseline (text-in / text-out). Most workflow skills.
2. **Capability strings** — skill depends on a runtime capability the host provides (e.g., `callable_agents`, `decisions_table`, `web_fetch`, `filesystem`).
3. **External library / dependency names** — when the skill ships scripts that import non-stdlib packages (use the package name).
4. **Environment variables** — when the skill expects specific environment variables to be present (use `env:VAR_NAME` shape).

For category 2 (runtime capabilities), include a short registry in SPEC §9 of canonical capability names so different skills don't invent divergent strings for the same dependency.

### Trigger to codify

Next public skill that declares more than one runtime capability OR the first private skill that does the same. Once two examples exist, the convention should be locked.

### Impact on this refactor

No behavioral change; this is a documentation gap, not a SPEC violation. `convening-council` ships SPEC v1.2-compliant with the proposed format. The `interface.yaml` `consumes:` block uses `type: capability` to surface the same dependencies for downstream tooling — that's a candidate SPEC clarification too (whether `type: capability` is a recognized type or just convention).

---

## Out of scope for W5

- **CAPABILITIES.md creation** (the SPEC §8 item 10 graceful-degradation target) — separate concern.
- **Cross-skill shared references** (W2 from the prior workflow-archetype PILOT_LEARNINGS) — different amendment.
- **Discovery / declaration of operator-provided capabilities** — runtime concern; SPEC handles declaration shape, not the runtime contract.

---

## References

- `creating-social-content/PILOT_LEARNINGS.md` — W1-W4 from the workflow-archetype pilot
- SPEC v1.2 §2.1, §8 item 7, §9
- The compliance gate (`requires: []` baseline check) — accepts the proposed list-of-strings format without modification
