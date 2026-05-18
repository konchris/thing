# Artifact Schema

Defines the expected structure of THING-generated systems engineering artifacts.

## Common Artifact Metadata

Use simple YAML front matter where the documentation system supports it.

```yaml
---
title: Concept of Operations
type: workflow
status: draft
owner: maintainers
last_reviewed: 2026-05-18
traces_to:
  - stakeholder-analysis
  - nfr
  - architecture
---
```

| Field | Meaning |
|---|---|
| `title` | Human-readable artifact title |
| `type` | `guide`, `concept`, `workflow`, `reference`, `example`, `decision` |
| `status` | `draft`, `stable`, `deprecated` |
| `owner` | Responsible maintainer or role |
| `last_reviewed` | Date of last deliberate review |
| `traces_to` | Upstream and downstream artifact links |

## Common Sections

Not every artifact requires every section. Workflow artifacts should strongly prefer this structure.

| Section | Purpose |
|---|---|
| Purpose | What engineering uncertainty this artifact reduces |
| Scope | What is included and excluded |
| Inputs | Required upstream artifacts or decisions |
| Outputs | Produced downstream artifacts |
| Workflow Steps | Numbered, actionable sequence |
| Quality Criteria | How to recognize adequate output |
| Traceability | Source, relationship, and target mapping |
| Decisions | Durable choices and rationale |
| Open Questions | Known unresolved issues requiring follow-up |
| Common Failure Modes | What bad versions of this artifact look like |

## Traceability Block

Every major artifact should end with a traceability block.

```markdown
## Traceability

| Source | Relationship | Target |
|---|---|---|
| STK-001 | motivates | CONOPS-OP-001 |
| CONOPS-OP-001 | constrains | NFR-REL-001 |
| NFR-REL-001 | verified by | VER-TEST-004 |
```

## ID Prefixes

| Prefix | Meaning |
|---|---|
| `STK` | Stakeholder |
| `CONOPS` | Concept of Operations |
| `NFR` | Non-functional requirement |
| `FR` | Functional requirement |
| `ARCH` | Architecture element |
| `IF` | Interface |
| `RISK` | Risk |
| `VER` | Verification item |
| `VAL` | Validation item |
| `ADR` | Architecture decision record |

Use IDs as handles, not decoration. A requirement without an ID cannot be traced.

## Callout Style

Use restrained callouts only when context requires it.

```markdown
> **Note**
> Use when the reader might otherwise misinterpret.

> **Decision**
> Records a choice that changes downstream work.

> **Risk**
> Marks an unresolved uncertainty requiring follow-up.

> **Traceability**
> Link this artifact to upstream intent and downstream verification.
```

Avoid emoji-driven callouts.

## Related Templates

- [`templates/artifact-template.md`](../../templates/artifact-template.md)
- [`templates/workflow-template.md`](../../templates/workflow-template.md)
- [Documentation Structure Identity](../conventions/documentation-structure-identity.md)
