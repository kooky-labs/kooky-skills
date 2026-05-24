<!--
AGENTS.md in this folder is a symlink to this file (CLAUDE.md). Same inode, two names — for Anthropic and OpenAI surfaces respectively. There is no "sync." If they appear divergent, the symlink was broken. Restore from this folder with:
    ln -sf CLAUDE.md AGENTS.md
-->

# kooky-skills — Canonical KOOKY Skill Repository

This repo is the **canonical store** for KOOKY-authored skills. Per ADR-015 D13 + D25, the canonical store must be network-reachable; this is where it lives.

**Renamed from `kooky-skills-approved` on 2026-05-24** per ADR-015 D13. GitHub redirects old URLs automatically.

## Standard

Every skill in `skills/` MUST satisfy:
- [ADR-017](/Users/renatosgafilho/Projects/KOOKY/company/kooky-os/knowledge/architecture/ADR-017-Skill-Standard-Locale-Aware-Interfaces-and-os-CLI.md) — Skill Standard architectural authority
- [SPEC](/Users/renatosgafilho/Projects/_OS/templates/skill-template/SPEC.md) — the reference contract (file shape, frontmatter, interface.yaml, i18n bundles)
- [VETTING.md](./VETTING.md) — security review checklist (mandatory for any skill touching secrets, network, or external data)

## Scope

Holds **KOOKY-authored skills only** (KOOKY's IP). Third-party skills (Anthropic-authored, Cowork-default, community-contributed) are not stored here — they live where their authors publish them.

Per ADR-017 D4, KOOKY-authored skills include:
- `os-<tool>` connector skills (direct implementations and adapters around third-party connectors)
- Unprefixed workflow / utility / meta-skills (`triage-inbox`, `wikify`, `skill-creator`, etc.)

## File map

- `README.md` — purpose + contribution flow
- `VETTING.md` — security review checklist
- `skills/<skill-name>/` — individual skills, each compliant with the SPEC
- `MEMORY.md` / `memory/` — repo-level state

## Runtime view

`~/Projects/_OS/skills/<skill-name>/` is the **runtime consumption view** — symlinks into this repo's `skills/` directory. Claude Code reads via `~/.claude/skills/` symlinks; Cowork reads `~/Projects/_OS/skills/` directly. The authoring source of truth lives here in kooky-skills; the symlinks expose it where consumers expect it.

## Channels (per ADR-015 D16) — deferred

The branch-as-channel model (`edge`, `beta`, `stable` with semver tags) is **deferred** until skill promotion volume justifies it. Current state: `main` branch only. Promotion to channels lands when TANK + WATSON workflows are operational.

## Conventions

- **Authorship boundary is hard.** Don't add third-party-authored content to this repo (per ADR-017 D4). If a third-party skill needs to participate in the os CLI capability namespace, author a thin KOOKY adapter (e.g., `os-stripe`) that wraps it.
- **Vetting is non-negotiable.** A skill that hasn't passed VETTING.md does not get merged.
- **Standard compliance is non-negotiable.** A skill that doesn't satisfy the SPEC's §8 compliance checklist does not get merged.

## Conflict resolution

If this file conflicts with ADR-017, the SPEC, or VETTING.md, those win. Ask Renato before overriding a locked architectural decision.
