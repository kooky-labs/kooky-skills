# kooky-skills

The canonical repository for KOOKY-authored skills.

**Renamed from `kooky-skills-approved` on 2026-05-24** per [ADR-015 D13](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-015-Skills-Architecture-WORKING-DRAFT.md). GitHub redirects old URLs.

---

## What this repo is

The official source-of-truth for **KOOKY-authored skills**. Every skill here:

1. Is authored by KOOKY (Renato, Claude under KOOKY direction, or future Cartel agents — never third-party).
2. Satisfies the KOOKY Skill Standard (see [ADR-017](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-017-Skill-Standard-Locale-Aware-Interfaces-and-os-CLI.md) and the [SPEC](/Users/renatosgafilho/Projects/_OS/templates/skill-template/SPEC.md)).
3. Has passed the [VETTING checklist](./VETTING.md) for security review.

Third-party skills (Anthropic's plugins, Cowork-default skills, community-contributed) live where their authors publish them — they are **not** mirrored here.

---

## Skill structure

Each skill lives under `skills/<skill-name>/` and satisfies the SPEC. Minimum mandatory shape:

```
skills/
  <skill-name>/
    SKILL.md          # required — identity + operator-facing brief
    interface.yaml    # required — consumes/exposes/capability_flags
    i18n/             # required if skill emits user-facing copy
      en.json
      pt-BR.json
    references/       # optional — exemplars, schemas, edge cases
    scripts/          # optional — frozen helper code
    assets/           # optional — raw materials (logos, templates)
    evals/            # optional — quality tests
```

Full schema details in the [SPEC](/Users/renatosgafilho/Projects/_OS/templates/skill-template/SPEC.md).

---

## Naming conventions (ADR-017 D4)

- **Connector skills** (KOOKY-authored): `os-<tool>` — direct implementations (e.g., `os-mercadopago`) or thin adapters around third-party skills (e.g., `os-stripe` wraps Anthropic's `stripe`).
- **Workflow / utility / meta-skills** (KOOKY-authored): unprefixed kebab-case — `triage-inbox`, `wikify`, `skill-creator`.
- **Third-party skills**: not in this repo. They keep their original names where they live.

---

## Runtime consumption

Claude Code, Codex, and Cowork consume skills through symlinks:

- `~/.claude/skills/<skill-name>` → `~/Projects/_OS/skills/<skill-name>` → `~/Projects/KOOKY/company/kooky-skills/skills/<skill-name>`
- `~/.agents/skills/<skill-name>` → same chain
- Cowork reads `~/Projects/_OS/skills/` directly

The `_OS/skills/` directory is the **runtime view**; this repo is the **authoring source of truth**.

---

## Contributing a skill

1. Author the skill at `skills/<skill-name>/` following the SPEC.
2. Run the SPEC §8 compliance checklist locally.
3. Open a pull request.
4. A reviewer works through [VETTING.md](./VETTING.md).
5. On merge: symlinks are updated (manually for now; will be automated when skill-creator is updated per KOO2-17).

Failed compliance or vetting blocks the merge.

---

## Channels — deferred

ADR-015 D16 specifies branch-as-channel model (`edge`, `beta`, `stable`) with semver tags. **Not yet operational.** Current branch model: `main` only. Channel infrastructure lands when promotion volume justifies it (TANK + WATSON workflows running per ADR-015 D6).

---

## Pre-installed skills

All base skills for the KOOKY Cartel agent fleet. Each skill is assigned to a specific agent.

| Skill | Agent | Description |
|-------|-------|-------------|
| [social-content](skills/social-content/SKILL.md) | HOUDINI (CMO) | Write platform-native social media posts for X, LinkedIn, TikTok, and Instagram |
| [copywriting](skills/copywriting/SKILL.md) | HOUDINI (CMO) | Write marketing copy: headlines, body copy, CTAs, email subject lines |
| [content-strategy](skills/content-strategy/SKILL.md) | HOUDINI (CMO) | Plan content calendars, suggest topics, align with business objectives |
| [sales-outreach](skills/sales-outreach/SKILL.md) | MAVERICK (Sales) | Write personalized cold outreach for LinkedIn and email |
| [lead-qualification](skills/lead-qualification/SKILL.md) | MAVERICK (Sales) | Evaluate and score incoming leads against ICP criteria |
| [quality-review](skills/quality-review/SKILL.md) | WATSON (QA) | Review content, code, and deliverables for quality standards |
| [skill-security-audit](skills/skill-security-audit/SKILL.md) | WATSON (QA) | Vet skills for security before installation |
| [delegation](skills/delegation/SKILL.md) | KAI (CEO) | Analyze requests and delegate to the right agent |
| [executive-summary](skills/executive-summary/SKILL.md) | KAI (CEO) | Summarize agent activity and generate progress reports |
| [technical-architecture](skills/technical-architecture/SKILL.md) | JARVIS (CTO) | Design system architecture and evaluate technical approaches |
| [mcp-integration](skills/mcp-integration/SKILL.md) | JARVIS (CTO) | Build MCP server tools, define schemas, connect external services |
| [product-design](skills/product-design/SKILL.md) | LEONARDO (CPO) | Define features, prioritize backlog, write user stories, design user flows |
| [operations-automation](skills/operations-automation/SKILL.md) | ALFRED (COO) | Design operational workflows and identify automation opportunities |
| [market-research](skills/market-research/SKILL.md) | SHERLOCK (Research) | Research competitors, market trends, and produce structured analysis |
| [financial-planning](skills/financial-planning/SKILL.md) | FINN (CFO) | Budget analysis, revenue projections, cost optimization, pricing strategy |
| [skill-management](skills/skill-management/SKILL.md) | TANK (Skills) | Curate, validate, and install skills; manage skill marketplace |
| [delivery-management](skills/delivery-management/SKILL.md) | SCOTTY (Delivery) | Track client deliverables, manage timelines, coordinate agent handoffs |
| [customer-success](skills/customer-success/SKILL.md) | SAM (CS) | Onboard users, handle support, gather feedback, track satisfaction |
| [knowledge-management](skills/knowledge-management/SKILL.md) | DEWEY (Knowledge) | Organize and maintain knowledge bases, ensure information currency |

---

## References

- [ADR-017 — Skill Standard](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-017-Skill-Standard-Locale-Aware-Interfaces-and-os-CLI.md)
- [ADR-015 — Skills Architecture (WORKING DRAFT)](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-015-Skills-Architecture-WORKING-DRAFT.md)
- [ADR-006 — Skills Namespacing](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-006-Skills-Namespacing.md)
- [ADR-004 — Skills-First Architecture](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-004-Skills-First-Architecture.md)
- [SPEC v1.0-draft](/Users/renatosgafilho/Projects/_OS/templates/skill-template/SPEC.md)
- [VETTING.md](./VETTING.md)
