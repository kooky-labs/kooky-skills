<!--
AGENTS.md in this folder is a symlink to this file (CLAUDE.md). Same inode, two names — for Anthropic and OpenAI surfaces respectively. If they appear divergent, the symlink was broken. Restore with:
    ln -sf CLAUDE.md AGENTS.md
-->

# kooky-skills

A public showcase of skills authored for Claude-based agent fleets. Demonstrates how I think about skill design — frontmatter discipline, scope boundaries, security review, agent-role assignment.

## What's here

Each skill in `skills/` is a discrete, installable unit with a `SKILL.md` manifest. The set is organized by agent role (see the pre-installed-skills table in README).

## What this repo is NOT

Not a production system. Not the operational source-of-truth for any live agent fleet. Skills here may evolve, get refactored, or be superseded — treat them as illustrative.

## File map

- `README.md` — overview + pre-installed skill table by agent
- `VETTING.md` — security review checklist used when authoring/accepting skills
- `skills/<skill-name>/` — individual skills
- `skills/_template/` — scaffold for new skills

## Conventions

- Each skill carries a `SKILL.md` with YAML frontmatter (name, description, optional version + requires).
- Security review (see VETTING.md) before any skill that touches secrets, networking, or external data.
- Skills are kept narrowly scoped — one job per skill.
