---
name: managing-knowledge
description: >
  Organizes knowledge bases, evaluates source quality on 5 weighted
  dimensions (authority/accuracy/currency/relevance/depth), enforces
  documentation standards, runs maintenance cadence (daily/weekly/
  monthly/quarterly), and deprecates outdated content. Use when adding
  sources, auditing documentation health, or maintaining information
  currency.
version: 1.0.0
owner: DEWEY
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - knowledge
  - documentation
  - workflow
---

# managing-knowledge

Workflow skill for maintaining well-organized, accurate, current knowledge bases. Read-only — produces evaluations, audits, deprecation plans; never deletes knowledge permanently.

## What this skill does

Takes a knowledge-management request (source evaluation / structure audit / gap analysis / deprecation) and returns: weighted source-quality scores with INCLUDE / EXCLUDE / CONDITIONAL verdicts, documentation-structure audits against standards, gap-analysis reports, and deprecation plans (always archive, never delete).

## When to use it

- Evaluating a new source for inclusion in a knowledge base.
- Auditing documentation for staleness or gaps.
- Organizing or restructuring knowledge base content.
- Deprecating outdated documents.

Do **not** use this skill when:

- The need is authoring new content — wrong scope; this skill manages, doesn't write.
- The deletion is permanent — never permanent; archive instead.
- The locale isn't supported (currently `en` only).

## How to use it

### Source evaluation

Score each source 1-5 on five weighted dimensions:

- **Authority (25%)** — creator credentials, peer review, institutional backing.
- **Accuracy (25%)** — evidence support, factual errors, cross-source alignment.
- **Currency (20%)** — publication date, ongoing relevance.
- **Relevance (15%)** — direct applicability to the organization's needs.
- **Depth (15%)** — thoroughness, actionable detail.

Minimum score for inclusion: 3.0 weighted average. Verdict: **INCLUDE / EXCLUDE / CONDITIONAL**.

### Documentation standards

- `<base>/agent-notes/` — Daily findings (`YYYY-MM-DD-<topic-slug>.md`)
- `<base>/architecture/` — ADRs and system design (`ADR-NNN-<title>.md`)
- `<base>/guides/` — How-to documents
- `<base>/research/` — Research outputs

(Folder names are conventions, not absolute paths — substitute your knowledge base root for `<base>`.)

### Maintenance cadence

- **Daily** — process new notes, tag, cross-reference.
- **Weekly** — review for outdated content.
- **Monthly** — gap analysis.
- **Quarterly** — full audit, archive deprecated content.

### Deprecation

Mark as needs-review, notify owner, allow a grace period, archive (never delete), update referencing documents.

## What to escalate

Stop and ask the operator before acting when:

- A source scores CONDITIONAL — operator decides whether to include with caveats or escalate to deeper review.
- Deprecation would orphan documents that still reference the deprecated content — surface the dependency before archiving.
- A critical knowledge gap appears in a domain the operator hasn't sourced — surface immediately rather than waiting for scheduled audit.
- The locale isn't supported (currently `en` only).

## Output format

- **Source evaluation** — 5-dimension weighted scores + verdict (INCLUDE / EXCLUDE / CONDITIONAL) + reasoning.
- **Audit report** — registry inventory with staleness flags, gap list, structure deviations.
- **Deprecation plan** — owner notification + grace period + archive location + referencing-doc update list.

## Constraints

- Never add sources without evaluation.
- Never delete knowledge permanently — archive deprecated content.
- Never skip cross-referencing — isolated documents lose value.
- Flag critical knowledge gaps immediately rather than waiting for scheduled review.

## References

- `interface.yaml` — machine-readable capability surface
- SPEC v1.2 §2.3 — operator-centric body section convention
