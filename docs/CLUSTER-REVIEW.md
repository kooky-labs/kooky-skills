# Cluster Review — Public Skills SPEC v1.2 Refactor

**Date:** 2026-05-24
**Author:** Cluster review pass before bulk refactor of the remaining 19 public skills
**Status:** Reviewed — all open questions resolved 2026-05-24. Cleared to commit + proceed with execution per revised batch order.

---

## TL;DR

**Final post-cluster skill count: 19 (no merges, no deletions, no splits).**

The cluster review surfaces a single substantive finding: every skill is coherent, complete, and well-scoped — there are no merge candidates, no deletion candidates, and no incomplete stubs. The original hypothesis that the content cluster (creating-social-content / copywriting / planning-content) overlapped substantially turns out to be wrong: the three skills cover three distinct surfaces (drafting social posts vs. drafting marketing copy fragments vs. scheduling). They share brand voice and pillar percentages but not job-to-be-done.

The one non-trivial refactor concern is `auditing-skill-security`: its "Quality Standards Check" section currently enforces an obsolete frontmatter rule (`ONLY name and description fields`) that would reject every SPEC v1.2-compliant skill. That section needs replacement, not just SPEC v1.2 frontmatter conversion. The security-audit portion of the skill (Tier A/B runtime checks: prompt injection, data exfiltration, scope creep, code safety, provenance) is not subsumed by the automated compliance checker and remains valuable.

**Headline verdicts by cluster:**

| Cluster | Verdict | One-liner |
|---|---|---|
| HOUDINI (copywriting, planning-content) | **keep all distinct; sharpen frontmatter** | Three skills, three distinct surfaces (social drafting / marketing copy / calendar scheduling). Pillar percentages duplicated across two skills — flag for SPEC v1.3, not a merge trigger. |
| KAI (delegating-tasks, summarizing-progress, convening-council) | **keep all distinct** | Routing vs. reporting vs. decision protocol. Three different jobs. |
| MAVERICK (conducting-outreach, qualifying-leads) | **keep both; sharpen handoff** | Sequential workflow (qualify → outreach). Make the handoff explicit in frontmatter. |
| WATSON (reviewing-quality, auditing-skill-security) | **keep both; HEAVY refactor on auditing-skill-security** | Its "Quality Standards Check" section enforces the obsolete v0/v1 frontmatter rule — must be replaced with a reference to SPEC v1.2 + the automated compliance checker. Security audit portion (Tier A/B) survives. |
| JARVIS (designing-architecture, building-integrations) | **keep both distinct** | Design vs. implement. Clean boundary. |
| Solo (8 skills) | **keep all** | Each is coherent and complete. SHERLOCK ↔ TANK boundary needs frontmatter sharpening but no merge. |

**Recommended batch order shift:** do WATSON first (the heavy refactor + dogfooding the skill that audits other skills), then the original sequence.

---

## HOUDINI — Content cluster

### Baseline (shipped, for comparison)

| Name | Purpose | Archetype | Quality | Notes |
|---|---|---|---|---|
| `creating-social-content` | Drafts platform-native social posts (X, LinkedIn, TikTok, Instagram) in founder voice. Output = post copy + metadata. | workflow | SPEC v1.2 compliant (shipped pilot) | Sets pillar percentages: Founder's Journey 40% / BTS 25% / Case Studies 20% / Hot Takes 15%. |

### To refactor

| Name | Purpose | Archetype | Quality | Overlap signals |
|---|---|---|---|---|
| `copywriting` | Drafts marketing copy fragments — headlines, body, CTAs, email subjects, ads, taglines. Output = copy fragments + A/B variants. | workflow | complete & useful | Shares brand voice + banned word list with `creating-social-content`. **Different surface** (landing pages, emails, ads — not social posts). |
| `planning-content` | Plans content calendars — weekly / monthly / quarterly. Output = scheduling tables, not copy. Pillar allocation + repurposing strategy. | workflow | complete & useful | Duplicates the 40/25/20/15 pillar percentages from `creating-social-content`. **Different job** (scheduling vs. drafting). |

### Verdict: **keep all 3 distinct; sharpen frontmatter cross-references**

The earlier hypothesis (cluster overlap from pilot findings) overstates the actual situation. The three skills cover three distinct jobs:

- **Drafting social posts** → `creating-social-content`
- **Drafting marketing copy fragments** → `copywriting`
- **Scheduling what gets drafted when** → `planning-content`

The shared brand voice + banned words + pillar percentages are appropriate shared vocabulary, not duplication. However, each skill's `description` field should explicitly state what it does NOT do, so an agent picks the right one:

- `copywriting.description` should add: "For social media posts, use `creating-social-content` instead."
- `planning-content.description` should add: "Plans — does not write content; use `copywriting` or `creating-social-content` for drafting."
- `creating-social-content` already excludes paid-ad copy in its body; cross-link in `description`.

The duplicated pillar percentages (40/25/20/15 appearing in two SKILL.md files) is a separate concern. Options: (a) leave it duplicated (each skill self-contained), (b) extract to a shared `references/brand-voice.md` and link from both. Recommend (a) for this refactor — keeps skills independent and copy-paste-installable. Flag as a possible SPEC v1.3 amendment (cross-skill shared references) but do not block on it.

---

## KAI — Orchestration cluster

| Name | Purpose | Archetype | Quality | Overlap signals |
|---|---|---|---|---|
| `delegating-tasks` | Classifies + routes incoming requests to agents. Agent routing table. Output = delegation with handoff sequence. | workflow | complete & useful | Uses the agent-codename vocabulary. No real overlap with siblings. |
| `summarizing-progress` | Generates executive summaries — daily briefings, weekly summaries, project status. Output = structured report. | workflow | complete & useful | Reports on what other skills produced. No structural overlap with siblings. |
| `convening-council` | Multi-agent decision protocol — parallel proposals, adversarial counter, decision row in `decisions` table. Output = one decision row. | workflow (protocol-shaped) | complete & useful | Most ambitious of the three — references `callable_agents`, `plan.log_action`, `decisions.create` (infrastructure assumed by the skill). |

### Verdict: **keep all 3 distinct**

Three different jobs with no merge case. Likely benefit from harmonizing interface vocabulary across the cluster:

- Agent identifiers (use the same handle format across delegating-tasks and convening-council — both already use uppercase codenames, good)
- Scope categories (Now / Today / This Week in delegating-tasks; priorities in summarizing-progress — different vocabularies for similar concepts; consider standardizing in `interface.yaml`)
- Output formats (delegating-tasks "delegation," summarizing-progress "report," convening-council "decision row" — distinct enough)

`convening-council` makes infrastructure assumptions (`callable_agents`, `decisions` table). The SPEC v1.2 `requires:` field should declare these as required runtime capabilities — likely the first public skill where `requires: []` becomes `requires: [callable_agents, decisions_table]` (verify shape during refactor; may need a new W-amendment if existing `requires:` examples don't fit).

---

## MAVERICK — Sales cluster

| Name | Purpose | Archetype | Quality | Overlap signals |
|---|---|---|---|---|
| `conducting-outreach` | Drafts personalized cold outreach (LinkedIn / email / follow-up sequences). Output = message ready to send. | workflow | complete & useful | Output (outreach copy) consumes input from `qualifying-leads` (the prospect data). |
| `qualifying-leads` | Scores leads on 5 weighted dimensions, assigns Hot/Warm/Cool/Cold tier, recommends next action. Output = qualification report + tier + actions. | workflow | complete & useful | Output (qualified prospect data) feeds `conducting-outreach`. |

### Verdict: **keep both distinct; sharpen capability boundary**

Sequential workflow with clean separation: qualify → outreach. No merge case. The `interface.yaml` for each should make the handoff explicit:

- `qualifying-leads.exposes.sales.qualify_lead.returns` should include prospect data shape that `conducting-outreach.consumes` can read.
- `conducting-outreach.consumes` should declare an optional `qualification_tier` parameter so it can vary message intensity by Hot/Warm/Cool (current SKILL.md doesn't formalize this — minor enhancement, not a blocker).

---

## WATSON — Quality cluster

| Name | Purpose | Archetype | Quality | Overlap signals |
|---|---|---|---|---|
| `reviewing-quality` | Reviews deliverables (content / code / agent outputs / business materials) on 5 dimensions, classifies findings BLOCK/MAJOR/MINOR/NOTE, verdict PASS/PASS WITH CHANGES/FAIL. | workflow | complete & useful | Operator-facing review tool. No overlap with the security audit. |
| `auditing-skill-security` | Vets skills before installation. Two-part check: Quality Standards (frontmatter rules) + Security Audit (Tier A/B runtime checks). | workflow / meta | **partially obsolete** — Quality Standards section enforces v0/v1 frontmatter rule (`ONLY name and description`) which now rejects every SPEC v1.2-compliant skill | Quality Standards section is now subsumed by SPEC v1.2 + the automated compliance checker. Security Audit portion (Tier A/B) is not subsumed. |

### Verdict: **keep both; HEAVY refactor on `auditing-skill-security`**

#### `reviewing-quality` — standard SPEC v1.2 conversion

No structural change beyond frontmatter + body restructure to operator-centric §2.3 sections.

#### `auditing-skill-security` — heavy refactor required

Current state contains contradictions with SPEC v1.2:

1. **"Quality Standards Check" section is obsolete.** It enforces "Frontmatter: ONLY `name` and `description` fields. No version, author, category, requires, or other fields." This is the v0/v1 rule. SPEC v1.2 mandates 9 required frontmatter fields. Following this skill's quality check would reject every SPEC v1.2-compliant skill including the shipped pilot.

2. **Format compliance is now automated.** The format-correctness portion of the quality check is the job of the automated compliance checker (covers structural SPEC §8 items 6-11). The skill should reference the automated checker rather than re-implementing partial format rules.

3. **Tier A/B security audit is NOT subsumed.** The four Tier A checks (prompt injection, data exfiltration, scope creep, code safety) and the three Tier B additions (provenance, dependency risk, adversarial) are runtime safety checks — not format checks. They remain the human-judgment layer that the automated checker explicitly doesn't cover.

**Refactor approach:**

- **Delete** the "Quality Standards Check" section entirely.
- **Replace** with a one-line note: "Format compliance is validated by the automated compliance checker; this skill picks up runtime security review after format validation passes."
- **Restructure** the rest under operator-centric §2.3 (what / when / how / escalate).
- **Keep** the Tier A / Tier B security audit logic — it's the skill's actual value-add.
- **Rename consideration:** the skill is now purely a security audit (quality compliance is delegated). Consider renaming `auditing-skill-security` → `auditing-skill-runtime` or keeping the name. Recommend keeping — "security" is the broader umbrella that includes the runtime-safety dimensions.

This is the heaviest refactor in the bulk batch. Estimate ~40-60 min for this one skill vs. ~15-20 min for a straight SPEC v1.2 conversion.

---

## JARVIS — Architecture cluster

| Name | Purpose | Archetype | Quality | Overlap signals |
|---|---|---|---|---|
| `designing-architecture` | System design + ADR authoring. 6-dimension evaluation (complexity, cost, speed, scalability, reversibility, dependencies). Output = ADR + design. | workflow | complete & useful | Design output is the input to `building-integrations` when the design specifies an MCP integration. No overlap. |
| `building-integrations` | Builds MCP server integrations — tool schemas, handlers, security, error handling, deployment. Output = working integration. | workflow / connector-author | complete & useful | Implements what `designing-architecture` specifies. No overlap. |

### Verdict: **keep both distinct**

Clean design-vs-implement boundary. Standard SPEC v1.2 conversions for both.

---

## Solo skills (8) — coherence check

Default verdict: keep each as-is, convert to SPEC v1.2.

| Name | Owner | Purpose | Archetype | Quality | Flags |
|---|---|---|---|---|---|
| `designing-products` | LEONARDO | Feature definition, backlog prioritization (ICE), user stories, user flows. Output = problem statement + stories + flows. | workflow | complete & useful | None. |
| `automating-operations` | ALFRED | Maps current-state workflows, classifies steps (automate fully / oversight / human / cannot), designs target state, implementation plan. Output = workflow design. | workflow | complete & useful | None. |
| `researching-skills` | SHERLOCK | Discover skills from external sources (skills.sh, GitHub, Claude Code global). Evaluate fit and security flags. Output = candidate list + recommendations. | workflow | complete & useful | **Boundary with TANK `managing-skills`** — see Open Questions. |
| `planning-finances` | FINN | Budgets, revenue projections (conservative/base/optimistic), cost optimization, pricing strategy. Output = analysis with ranges and sensitivity. | workflow | complete & useful | None. |
| `managing-skills` | TANK | Skill lifecycle: discovery, evaluation, security gate, installation, monitoring, removal. Output = installation report / recommendation / audit / search. | workflow / meta | complete & useful | **Boundary with SHERLOCK `researching-skills`** — see Open Questions. |
| `managing-delivery` | SCOTTY | Client project tracking across 4 phases (kickoff, execution, QA, go-live). Handoff protocols. Output = project tracker + blockers + risks. | workflow | complete & useful | None. |
| `managing-customer-success` | SAM | Onboarding (Day 1-30), support triage (P1-P4 SLAs), churn intervention. Output = health check / support response / churn plan. | workflow | complete & useful | None. |
| `managing-knowledge` | DEWEY | Source evaluation (5-dim weighted score), documentation standards, maintenance cadence (daily/weekly/monthly/quarterly), deprecation. Output = source verdict / audit. | workflow | complete & useful | None. |

**No deletions, no merges, no splits.** All 8 are coherent units with clear scope.

---

## Decisions Recorded (2026-05-24)

| # | Question | Decision | Notes |
|---|---|---|---|
| Q1 | `auditing-skill-security` refactor approach | **(A) Refactor in-place** | Keep the name; rip out the obsolete "Quality Standards Check" section; preserve Tier A/B runtime security checks. |
| Q2 | HOUDINI duplicated pillar percentages | **(A) Leave duplicated** + **reweight to 2026-Q2 campaign mix** | New mix: Case Studies 35% / Hot Takes 30% / Founder's Journey 20% / BTS 15%. Time-tagged as a campaign-period mix; scheduled revisit after 2026-06-26. Cross-skill shared references deferred to a future SPEC amendment. |
| Q3 | SHERLOCK ↔ TANK boundary | **(A) Sharpen descriptions only** | SHERLOCK = discover + pre-screen; TANK = lifecycle + security gate. No restructuring. |
| Q4 | Final post-cluster count | **19 confirmed** | Zero merges, zero deletions, zero splits. Plus the already-shipped pilot = 20 total in `kooky-skills/skills/`. |

These decisions are baked into the paired execution document — the HOUDINI task carries the pillar reweight specifics; the WATSON task carries the `auditing-skill-security` heavy-refactor scope; the Solo task carries the SHERLOCK/TANK description-sharpening guidance.

---

## Open Questions for Operator Review

### Q1 — `auditing-skill-security` refactor approach: in-place vs. rename + new skill?

The obsolete "Quality Standards Check" section needs to be removed. Two paths:

- **(A) Refactor in place** — same name, same dir, replace the section. Pros: stable identifier, no consumers to migrate. Cons: the skill's identity shifts from "audit + quality check" to "pure security audit," which the name no longer signals as precisely.
- **(B) Delete + create fresh** — new skill `auditing-skill-runtime` or similar; delete old. Pros: cleaner identity match. Cons: any consumers (`managing-skills` references skill auditing for the security gate; review the chain) need migration; net work is higher.

**Recommendation:** (A) refactor in place. The skill's actual job description is "runtime security audit for skills" — "auditing-skill-security" already names that accurately. The obsolete quality-check section was always secondary to the security audit; removing it sharpens the skill rather than redefining it.

### Q2 — HOUDINI cluster: duplicate pillar percentages — refactor now or defer?

The 40/25/20/15 pillar percentages appear in both `creating-social-content` and `planning-content` SKILL.md bodies. Options:

- **(A) Leave duplicated** — each skill remains self-contained and copy-paste-installable. (Current state on `creating-social-content` after the pilot refactor.)
- **(B) Extract to shared reference** — e.g., `kooky-skills/references/brand-voice.md`, link from both skills. SPEC v1.2 doesn't currently have a shared-references convention.

**Recommendation:** (A) for this refactor. (B) is a SPEC v1.3 conversation about cross-skill shared assets. Flag as a future W-amendment proposal during execution if other refactors surface more duplication.

### Q3 — SHERLOCK ↔ TANK boundary: sharpen frontmatter only, or restructure?

`researching-skills` (SHERLOCK) and `managing-skills` (TANK) overlap on the "evaluate for fit" subtask:

- SHERLOCK: "Evaluate each candidate" with fit + compatibility + trust level + security flags.
- TANK: "Evaluation: Initial screening — does it fill a real gap? Is there overlap with existing skills?"

They're complementary in intent (discover vs. lifecycle) but the evaluation step appears in both. Options:

- **(A) Sharpen descriptions only.** SHERLOCK = "discover candidates externally and pre-screen for fit." TANK = "lifecycle management of installed skills, including the security gate before install."
- **(B) Pull "evaluation" out of one** to be the other's responsibility. Cleaner separation but breaks the natural sequence each skill describes today.

**Recommendation:** (A). The evaluation steps are doing different work (SHERLOCK = "is this worth bringing in?"; TANK = "is this safe to install?") even if the section headers look similar. Frontmatter sharpening + cross-references is enough.

### Q4 — Final post-cluster count: 19 confirmed?

Verifying the conclusion: cluster review surfaces zero merges, zero deletions, one heavy refactor (auditing-skill-security). Net skill count after the bulk refactor: 20 total in `kooky-skills/skills/` (the previously shipped `creating-social-content` + the 19 refactored). The refactor plan's "14-19" estimate should be updated to a firm **19**.

---

## Recommended Batch Order Revision

Original plan sequence: HOUDINI → KAI → MAVERICK → WATSON → JARVIS → Solo.

**Revised sequence (rationale below):**

1. **WATSON first** (~50-70 min total). The heavy refactor of `auditing-skill-security` is the single largest unknown; resolve it early. `reviewing-quality` rides along as a standard conversion.
2. **HOUDINI next** (~30-40 min total). Two straight conversions; light cross-reference sharpening in descriptions. Validates that the shipped pilot's section convention generalizes.
3. **KAI** (~50-70 min total). Three skills; `convening-council` may surface a new W-amendment around `requires:` declaring runtime infrastructure (`callable_agents`, `decisions` table).
4. **MAVERICK** (~30-40 min total). Two skills with explicit capability handoff between them.
5. **JARVIS** (~30-40 min total). Two clean conversions.
6. **Solo (8 skills)** (~2-3 hours total). Subagent parallelism trial. Calibrate with one inline conversion (recommend starting with `planning-finances` — clean, no overlap concerns, predictable scope). If < 15 min, continue inline; else dispatch the remaining 7 in parallel worktrees.

**Reason for putting WATSON first:** dogfooding. Once `auditing-skill-security` is refactored to delegate format compliance to the automated checker, every subsequent skill in the batch gets audited under the post-refactor rules. Doing WATSON last would mean either (a) skipping audits on the first 16 skills, or (b) auditing them under the obsolete rules. Doing it first means the bulk batch can declare "passes both `auditing-skill-security` runtime checks AND the automated compliance gate" as a single coherent statement.

**Cluster commit strategy** (per the plan): one commit per cluster. WATSON's commit will be larger than the others because of the auditing-skill-security refactor. JARVIS and MAVERICK can fit in single commits. Solo skills batch into 2-4 commits depending on subagent dispatch outcomes.

---

## What this review did NOT do

- **Did not modify any SKILL.md files** — read-only audit.
- **Did not modify `agent-mapping.json` or `README.md`** — those update at execution time after final cluster verdict.
- **Did not commit** — operator reviews this document first.
- **Did not run the automated compliance checker** on any of the 19 — that's per-skill work during execution.
- **Did not check for cross-skill references** in body text beyond the obvious pillar-percentage duplication — execution-time concern.

---

## Sources

- All 19 SKILL.md files in `kooky-skills/skills/<name>/` read in full.
- Shipped reference: `kooky-skills/skills/creating-social-content/SKILL.md` (SPEC v1.2 compliant).
- Agent ownership input: `kooky-skills/skills/agent-mapping.json`.
- Repo conventions: `kooky-skills/CLAUDE.md`, `kooky-skills/README.md` (table of skills by agent — referenced, not modified).
