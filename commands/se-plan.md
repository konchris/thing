---
description: "Generate the SE feature plan from ConOps, NFRs, explore convergence decision, and requirements register. Produces 07-plan/plan.md — the technical planning artifact that se-architecture reads. This skill replaces /speckit-plan for SE features, which have no spec.md."
---

# SE Plan

## Purpose

Produce the technical plan for an SE feature by synthesising the
operational picture (ConOps), quality obligations (NFRs), selected
solution approach (explore convergence), and requirements baseline.
The plan is the bridge between problem-space understanding and
architecture — it describes *how the system will be built* at a
level above individual design decisions.

This skill replaces `/speckit-plan` for SE features. The standard
`/speckit-plan` reads `spec.md` (which SE features do not have)
and writes to the feature root rather than `07-plan/`. This skill
reads the SE artifact set and writes to the correct location.

The plan produced here is the primary input to
`/thing.se-architecture`. Do not run `/se-architecture`
until this plan exists.

Output artifacts:

- `specs/features/$FEATURE_ID/07-plan/plan.md`

Updates:

- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it.
- `specs/system/01-conops/conops.md` must exist. Read it.
- `specs/system/02-nfr/quality-attributes.md` must exist.
  Read it. Must Have NFRs are architectural drivers and must be
  explicitly addressed in the plan.
- `specs/system/04-requirements/requirements-register.md`
  must exist. Read it. The plan must be feasible against the
  complete requirements set.
- `specs/features/$FEATURE_ID/03-explore/explore-summary.md` — read
  if it exists. The selected option's technical approach is the
  starting point for planning.
- `specs/system/10-risk/risk-register.md` — read if it exists.
  Open technical risks must be accounted for in the plan.

If the first four pre-conditions are not met, stop and tell the
user which skills must be run first.

## Instructions

### Step 1 — Read and Internalise Context

Read all pre-condition artifacts. Build an internal picture of:

- The system's mission, boundary, and constraints (constitution)
- The operational modes and key scenarios (ConOps)
- The Must Have and Should Have NFRs — these are non-negotiable
  architectural constraints
- The full requirements set and any coverage gaps still open
- The selected solution option and its technical approach (explore)
- Any open technical risks

Do not begin writing until you have read all available artifacts.

### Step 2 — Establish the Technical Approach

Synthesise the selected explore option (if present) with the
requirements and NFRs to establish the technical approach.

If no explore option was selected (se-explore was not run),
derive the technical approach from the ConOps operational modes
and the Must Have requirements.

The technical approach must answer:

1. What is the system decomposed into? Identify the major
   subsystems, services, or components needed to satisfy the
   requirements. Name them — these become the architecture's
   primary decomposition.
2. How do the components interact? At a high level, what are
   the key flows between components?
3. Which NFRs drive structural decisions? For each Must Have
   NFR, state the architectural pattern or mechanism that will
   satisfy it (e.g., "99.9% availability → active-passive
   redundancy with automatic failover").
4. What technology choices are already constrained? By the
   constitution, existing systems, or the explore convergence
   decision.
5. What technology choices remain open? These are decisions
   for se-architecture to resolve.

### Step 3 — Identify Open Technical Questions

List the technical questions that the architecture phase must
resolve before implementation can begin. These are questions
where the answer materially affects the design — not minor
implementation details.

Format each as:

- **Question:** What specifically is unknown?
- **Why it matters:** Which requirement or NFR does the answer affect?
- **Options known so far:** Any candidate approaches already considered
- **Resolution method:** How will it be resolved? (spike, prototype,
  research, expert consultation, vendor evaluation)

### Step 4 — Define the Implementation Sequence

Based on the requirements and their priorities, define the
intended implementation sequence:

1. Which components or capabilities must be built first?
   (Typically: foundational infrastructure, then core functional
   requirements, then quality obligations, then optional features)
2. Are there dependencies between components that dictate order?
3. Are there requirements with external dependencies (third-party
   integrations, hardware deliverables, regulatory approvals) that
   create schedule constraints?
4. What is the minimum viable implementation that satisfies all
   Must Have requirements?

### Step 5 — Write the Plan

Create directory `specs/features/$FEATURE_ID/07-plan/` if it does not exist.

Write `specs/features/$FEATURE_ID/07-plan/plan.md`:

```markdown
# Feature Plan

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** DRAFT
**Last Updated By:** se-plan skill

---

## 1. Technical Approach

### System Decomposition

{Describe the major components or subsystems. For each:}

**{Component Name}**
- Purpose: {what it does}
- Primary requirements it satisfies: {FR-NNN, FR-NNN}
- NFRs it must respect: {NFR-NNN}

### Key Interactions

{Describe the primary flows between components. Use plain language —
diagrams will be produced in /se-architecture.}

### NFR-Driven Design Decisions

| NFR ID | Attribute | Mechanism |
| ------ | --------- | --------- |
| NFR-001 | {attribute} | {architectural pattern or mechanism} |

### Technology Constraints

{List technology choices already fixed by the constitution,
existing systems, or the explore convergence decision.}

| Constraint | Technology/Platform | Source |
| ---------- | ------------------- | ------ |
| Language | {language and version} | Constitution |

---

## 2. Open Technical Questions

{Questions the architecture phase must resolve.}

| ID | Question | Affects | Options | Resolution Method |
| -- | -------- | ------- | ------- | ----------------- |
| OTQ-001 | {question} | FR-NNN, NFR-NNN | {options} | {how to resolve} |

---

## 3. Implementation Sequence

### Phase Breakdown

**Phase 1 — Foundation**
{What must be built first. Prerequisites for everything else.}
Requirements addressed: {FR-NNN, ...}

**Phase 2 — Core Functionality**
{Primary functional requirements.}
Requirements addressed: {FR-NNN, ...}

**Phase 3 — Quality Obligations**
{NFR-driven mechanisms, error handling, edge cases.}
Requirements addressed: {FR-NNN, NFR-NNN, ...}

**Phase 4 — Should Have Features**
{Secondary requirements.}
Requirements addressed: {FR-NNN, ...}

### Minimum Viable Implementation

{The subset of requirements that constitutes the minimum
acceptable system — satisfies all Must Have requirements
and passes all Must Have NFRs.}

Requirements: {FR-NNN, ...}

### External Dependencies and Schedule Constraints

| Dependency | Type | Affects | Notes |
| ---------- | ---- | ------- | ----- |
| {name} | Third-party / Hardware / Approval | {FR or component} | {notes} |

---

## 4. Requirements Coverage Check

| Req ID | Addressed By | Phase | Notes |
| ------ | ------------ | ----- | ----- |
| FR-001 | {component} | {phase} | |

Uncovered requirements (must be resolved before architecture gate):
{list or "None — all requirements are addressed"}

---

## 5. Risks Introduced by This Plan

{Technical risks arising from planning decisions — approaches
that carry uncertainty, dependencies that are unverified,
assumptions that could invalidate the plan.}

See `10-risk/risk-register.md` for full risk entries.

| Risk ID | Description | Mitigation in Plan |
| ------- | ----------- | ------------------ |
```

### Step 6 — Update Risk Register

Add entries to `specs/system/10-risk/risk-register.md` for:

- Any open technical question (OTQ) where the answer could
  materially change the design
- Any external dependency that is not yet confirmed
- Any NFR whose mechanism is not yet validated
- Any assumption in the technical approach that could be wrong

### Step 7 — Confirm Completion

Report to the user:

1. Number of components or subsystems identified
2. Number of open technical questions recorded
3. Number of implementation phases defined
4. Requirements coverage: N of M requirements addressed in the plan
   (list any uncovered)
5. Number of risk entries added
6. Recommended next skill: `/thing.se-architecture`
