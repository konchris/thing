# THING Documentation Structure Identity

THING’s documentation is not supporting material.

For this project, the documentation *is* a major part of the product.

The identity should emerge from disciplined structure:

- clear artifact boundaries
- stable heading hierarchy
- restrained visual conventions
- repeatable requirements notation
- traceability-forward layout
- calm diagram language
- explicit decision records
- verification hooks

The goal is not “pretty documentation.”

The goal is documentation that feels like an engineering instrument.

---

## Core Rule

Every document should answer three questions quickly:

1. **What is this artifact for?**
2. **What decision or engineering object does it govern?**
3. **What downstream artifacts should trace back to it?**

If a document cannot answer those, it is probably prose rather than an engineering artifact.

---

## Recommended Documentation Tree

```text
docs/
  index.md

  guide/
    quick-start.md
    workflow-overview.md
    using-with-ai-agents.md

  concepts/
    systems-engineering.md
    thing-assembly-model.md
    traceability.md
    verification-thinking.md

  workflows/
    stakeholder-analysis.md
    conops.md
    nfr.md
    architecture.md
    interfaces.md
    traceability.md
    verification-validation.md
    risk-management.md

  reference/
    artifact-schema.md
    command-catalog.md
    glossary.md

  examples/
    minimal-system/
      README.md
      stakeholder-analysis.md
      conops.md
      nfr.md
      architecture.md
      traceability.md
      verification.md
      risk-register.md

  decisions/
    adr-0001-record-architecture-decisions.md
```

---

## Page Types

THING should distinguish between different types of documentation.

| Page Type | Purpose | Tone |
|---|---|---|
| Guide | Teach use | clear, procedural |
| Concept | Explain mental model | precise, reflective |
| Workflow | Define repeatable engineering activity | normative, structured |
| Reference | Stable lookup | terse, exact |
| Example | Show a worked artifact | concrete, minimal |
| Decision Record | Preserve rationale | explicit, dated |

Avoid mixing these page types.

A page that tries to teach, justify, specify, and exemplify all at once usually becomes noisy.

---

## Standard Page Front Matter

Use simple YAML front matter where the documentation system supports it.

```yaml
---
title: Concept of Operations Workflow
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

Recommended fields:

| Field | Use |
|---|---|
| `title` | Human-readable page title |
| `type` | `guide`, `concept`, `workflow`, `reference`, `example`, `decision` |
| `status` | `draft`, `stable`, `deprecated` |
| `owner` | Maintainer or owning group |
| `last_reviewed` | Date of last deliberate review |
| `traces_to` | Upstream/downstream artifact links |

---

## Heading Hierarchy

Use a stable hierarchy.

```markdown
# Artifact or Page Title

## Purpose

## Inputs

## Outputs

## Workflow

## Quality Criteria

## Traceability

## Examples

## Related Artifacts
```

Not every page needs every section, but workflow pages should strongly prefer this structure.

---

## Standard Workflow Page Structure

Each workflow document should follow this pattern:

```markdown
# <Workflow Name>

## Purpose

What engineering uncertainty this workflow reduces.

## When to Use

The trigger conditions for this workflow.

## Inputs

What artifacts, context, or decisions are needed before starting.

## Outputs

What durable artifacts should exist afterward.

## Workflow Steps

1. Step one
2. Step two
3. Step three

## Quality Criteria

How to recognize whether the output is good enough.

## Traceability Hooks

Which upstream and downstream artifacts should link here.

## Common Failure Modes

What bad versions of this artifact look like.

## Example

A small worked example.
```

---

## Requirement Formatting

Use stable IDs.

```markdown
### NFR-PERF-001 — Response latency

The system shall complete operator-visible workflow transitions within 500 ms under nominal operating conditions.

**Rationale:** Prevents perceived interface lag during supervised operation.

**Verification:** Test

**Traces to:** CONOPS-OP-003, ARCH-COMP-002
```

Recommended ID prefixes:

| Prefix | Meaning |
|---|---|
| `STK` | Stakeholder |
| `CONOPS` | Concept of Operations |
| `NFR` | Non-functional requirement |
| `ARCH` | Architecture element |
| `IF` | Interface |
| `RISK` | Risk |
| `VER` | Verification item |
| `VAL` | Validation item |
| `ADR` | Architecture decision record |

Use IDs as handles, not decoration.

---

## Callout Style

Use restrained callouts.

```markdown
> **Note**  
> Use this when context prevents misinterpretation.

> **Decision**  
> This records a choice that changes downstream work.

> **Risk**  
> This marks an unresolved uncertainty that requires follow-up.

> **Traceability**  
> Link this artifact to upstream intent and downstream verification.
```

Avoid emoji-driven callouts. They create the wrong tone.

---

## Tables

Use tables for structure, not decoration.

Good uses:

- requirement attributes
- stakeholder concerns
- interface fields
- risk register entries
- verification mapping
- architecture tradeoffs

Avoid large prose tables that become unreadable in GitHub.

---

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

This makes the systems engineering logic visible.

---

## Common Failure Modes

THING documentation should explicitly name failure modes.

Examples:

| Failure Mode | Symptom |
|---|---|
| Prompt pack drift | Commands exist, but artifacts do not connect |
| Requirements theater | Requirements are written but not verifiable |
| Architecture theater | Diagrams exist without decisions |
| Traceability cosplay | Links exist but do not carry reasoning |
| AI overproduction | Agent produces large prose instead of useful engineering artifacts |
| Premature implementation | Code starts before intent is structured |

This fits the identity: serious, slightly dry, quietly sharp.

---

## File Naming

Use lower-case kebab-case.

Good:

```text
stakeholder-analysis.md
concept-of-operations.md
verification-validation.md
risk-register.md
```

Avoid:

```text
Stakeholder Analysis Final v2.md
CONOPS_NEW.md
architecture_notes_chris.md
```

---

## Tone Rules

THING documentation should sound like:

- an engineering handbook
- a Unix manual
- a systems architecture note
- a disciplined project notebook

It should not sound like:

- a SaaS landing page
- an AI prompt marketplace
- an enterprise consulting deck
- a gaming manual
- a mystical Norse brand manifesto

Use short declarative sentences.

Prefer:

```text
Use this workflow before architecture decisions are made.
```

over:

```text
Unlock next-generation clarity by leveraging structured workflows before entering your architecture journey.
```

---

## Documentation Identity Principle

The identity is visible when a reader feels:

> This project governs complexity before it generates code.

That should be true in every artifact.
