# kooky-skills

A public showcase of skills authored for Claude-based agent fleets. Demonstrates skill design discipline: narrow scope, security review, role-based assignment.

This repository is **illustrative**, not production. Skills here may change, get refactored, or be superseded. It's a snapshot of how I work with skills — not the operational system any live fleet runs on.

---

## Skill structure

Each skill lives under `skills/<skill-name>/` and follows this layout:

```
skills/
  <skill-name>/
    SKILL.md        # required — YAML frontmatter + instructions
    run.sh          # optional — executable script (chmod 755)
    README.md       # optional — human-readable documentation
```

`SKILL.md` frontmatter:

```yaml
---
name: skill-name
description: One-line description of what this skill does
version: 1.0.0
author: kooky-labs
requires: []        # system deps, e.g. ["ffmpeg", "python3"]
---
```

---

## Pre-installed skills

A reference fleet of skills mapped to typical agent roles. Showcase only — adapt to your own fleet.

| Skill | Role | Description |
|-------|-------|-------------|
| [social-content](skills/social-content/SKILL.md) | CMO | Write platform-native social media posts for X, LinkedIn, TikTok, and Instagram |
| [copywriting](skills/copywriting/SKILL.md) | CMO | Write marketing copy: headlines, body copy, CTAs, email subject lines |
| [content-strategy](skills/content-strategy/SKILL.md) | CMO | Plan content calendars, suggest topics, align with business objectives |
| [sales-outreach](skills/sales-outreach/SKILL.md) | Sales | Write personalized cold outreach for LinkedIn and email |
| [lead-qualification](skills/lead-qualification/SKILL.md) | Sales | Evaluate and score incoming leads against ICP criteria |
| [quality-review](skills/quality-review/SKILL.md) | QA | Review content, code, and deliverables for quality standards |
| [skill-security-audit](skills/skill-security-audit/SKILL.md) | QA | Vet skills for security before installation |
| [delegation](skills/delegation/SKILL.md) | CEO | Analyze requests and delegate to the right agent |
| [executive-summary](skills/executive-summary/SKILL.md) | CEO | Summarize agent activity and generate progress reports |
| [technical-architecture](skills/technical-architecture/SKILL.md) | CTO | Design system architecture and evaluate technical approaches |
| [mcp-integration](skills/mcp-integration/SKILL.md) | CTO | Build MCP server tools, define schemas, connect external services |
| [product-design](skills/product-design/SKILL.md) | CPO | Define features, prioritize backlog, write user stories, design user flows |
| [operations-automation](skills/operations-automation/SKILL.md) | COO | Design operational workflows and identify automation opportunities |
| [market-research](skills/market-research/SKILL.md) | Research | Research competitors, market trends, and produce structured analysis |
| [financial-planning](skills/financial-planning/SKILL.md) | CFO | Budget analysis, revenue projections, cost optimization, pricing strategy |
| [skill-management](skills/skill-management/SKILL.md) | Skills | Curate, validate, and install skills; manage skill marketplace |
| [delivery-management](skills/delivery-management/SKILL.md) | Delivery | Track client deliverables, manage timelines, coordinate agent handoffs |
| [customer-success](skills/customer-success/SKILL.md) | CS | Onboard users, handle support, gather feedback, track satisfaction |
| [knowledge-management](skills/knowledge-management/SKILL.md) | Knowledge | Organize and maintain knowledge bases, ensure information currency |

---

## Contributing

1. Fork this repo
2. Copy `skills/_template/` to `skills/<your-skill-name>/`
3. Fill in `SKILL.md` and (if needed) `run.sh`
4. Open a pull request
5. A reviewer will work through [VETTING.md](./VETTING.md) before merging

Skills that fail any critical check in VETTING.md will be rejected.

---

## License + scope

Public, illustrative. See repo settings for license. This is a personal showcase and not a substitute for production skill management.
