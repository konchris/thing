# THING — Workflow Reference

THING supports four workflow paths. This document describes Path A (Full SE)
in detail. For the complete path-selection guide and sequences for all four
paths, see [thing-architecture.md](thing-architecture.md).

## Overview

This extension adds SE Handbook-aligned process to Spec-Kit's Spec-Driven Development workflow. It introduces two categories of artifact: **sequenced** (created once at a defined phase) and **threaded** (initiated at a specific phase, updated continuously until project close).

## Artifact Categories

### Sequenced Artifacts

Created at a defined phase. Have a clear completion state. Consumed as inputs by later phases.

### Threaded Artifacts

Initiated at a specific phase. Never "done" until project closes. Updated as part of completing each subsequent sequenced skill.

| Artifact | Initiated | Updated at |
| --- | --- | --- |
| `constitution.md` | Phase 0 | Any phase that changes governing principles |
| `risk-register.md` | Phase 0 | Every phase |
| `vv-plan.md` | Phase 1 | Phase 2 (per requirement), Phase 3, Phase 6 |
| `rtm.md` | Phase 2 | Phase 3, 4, 5, 6 |

## Workflow

```text
PHASE 0 — PROBLEM SPACE
/thing.se-constitution   (enhances base constitution)
/thing.se-stakeholders   (personas, empathy maps, needs register)
/thing.se-conops         (operational scenarios, modes, ConOps)
THREAD INITIATIONS:
→ constitution.md created/updated
→ risk-register.md created with assumption-based seed entries

PHASE 1 — SOLUTION SPACE
/thing.se-nfr            (quality attributes, constraints, regulatory)
/thing.se-explore        (set-based design, parallel options)
THREAD INITIATIONS:
→ vv-plan.md created with NFR entries
→ risk-register.md updated with technical unknowns
GATE: /thing.se-gate 1

PHASE 2 — REQUIREMENTS
/thing.se-requirements   (derive FRs from ConOps/NFRs/explore, IEEE 29148 quality checks)
/thing.se-req-quality    (IEEE 29148 requirements quality gate)
THREAD UPDATES:
→ rtm.md created, one row per requirement
→ vv-plan.md gets one entry per requirement
→ risk-register.md reviewed against completed spec
GATE: /thing.se-gate 2

PHASE 3 — ARCHITECTURE
/thing.se-plan           (SE feature plan from ConOps, NFRs, explore artifacts → 07-plan/plan.md)
/thing.se-architecture   (C4+Mermaid views, ADRs)
/thing.se-interfaces     (Interface Control Document)
THREAD UPDATES:
→ rtm.md: design element allocations added
→ vv-plan.md: verification stages updated
→ risk-register.md: technical risks from architecture added
GATE: /thing.se-gate 3

PHASE 4 — IMPLEMENTATION PLANNING
/speckit-tasks
/speckit-analyze
THREAD UPDATES:
→ rtm.md: task references added
GATE: /thing.se-gate 4

PHASE 5 — IMPLEMENTATION
/speckit-implement
THREAD UPDATES:
→ rtm.md: implementation references added
GATE: /thing.se-gate 5

PHASE 6 — VERIFICATION & VALIDATION
/thing.se-vvplan         (finalise V&V plan)
/thing.se-rtm            (finalise RTM)
/thing.se-risk           (close or carry forward risks)
THREAD CLOSURE:
→ vv-plan.md: all results populated
→ rtm.md: coverage 100% confirmed
→ risk-register.md: all risks dispositioned
GATE: /thing.se-gate 6 (final)
```

## Repository Structure

System-level SE artifacts live at `specs/system/` and are updated in
place with each feature iteration. Feature-scoped artifacts are created
fresh per feature at `specs/features/{feature-id}/`.

```text
specs/
├── system/                        # System-level — one copy, updated each feature
│   ├── 00-stakeholders/
│   │   ├── stakeholder-register.md
│   │   ├── personas/
│   │   ├── empathy-maps/
│   │   └── needs-register.md
│   ├── 01-conops/
│   │   ├── conops.md
│   │   └── operational-scenarios.md
│   ├── 02-nfr/
│   │   └── quality-attributes.md
│   ├── 04-requirements/
│   │   ├── requirements-register.md
│   │   └── req-quality-report.md
│   ├── 05-architecture/
│   │   ├── architecture.md
│   │   ├── views/
│   │   │   ├── context.md
│   │   │   ├── container.md
│   │   │   ├── component.md
│   │   │   ├── functional-flow.md
│   │   │   └── deployment.md
│   │   └── decisions/
│   │       └── adr-{NNN}-{title}.md
│   ├── 06-interfaces/
│   │   └── icd.md
│   ├── 09-vv/
│   │   ├── vv-plan.md
│   │   └── vv-results.md
│   ├── 10-risk/
│   │   └── risk-register.md
│   └── rtm.md
└── features/
    └── {feature-id}-{feature-name}/  # Feature-scoped — one set per feature
        ├── 03-explore/
        │   ├── explore-summary.md
        │   ├── option-{id}/
        │   │   ├── description.md
        │   │   ├── assumptions.md
        │   │   ├── tasks.md
        │   │   └── evaluation-log.md
        │   └── elimination-record.md
        ├── 07-plan/
        │   └── plan.md
        ├── 08-tasks/
        │   └── tasks.md
        └── gate-reports/
```

## Inter-Skill Input/Output Contract

System artifacts are always at `specs/system/`. Feature artifacts are at `specs/features/$FEATURE_ID/`.

| Skill | Reads | Writes |
| --- | --- | --- |
| `se-constitution` | (user input) | `constitution.md`, `specs/system/10-risk/risk-register.md` (seed) |
| `se-stakeholders` | `constitution.md` | `specs/system/00-stakeholders/`, `specs/system/10-risk/risk-register.md` (updated) |
| `se-conops` | `specs/system/00-stakeholders/`, `constitution.md` | `specs/system/01-conops/` |
| `se-nfr` | `specs/system/01-conops/`, `specs/system/00-stakeholders/needs-register.md` | `specs/system/02-nfr/quality-attributes.md`, `specs/system/09-vv/vv-plan.md` (seed) |
| `se-explore` | `specs/system/00-stakeholders/needs-register.md`, `specs/system/02-nfr/quality-attributes.md` | `specs/features/$FEATURE_ID/03-explore/` |
| `se-requirements` | `specs/system/01-conops/`, `specs/system/02-nfr/`, `specs/features/$FEATURE_ID/03-explore/explore-summary.md` | `specs/system/04-requirements/requirements-register.md`, `specs/system/09-vv/vv-plan.md`, `specs/system/rtm.md` |
| `se-req-quality` | `specs/system/04-requirements/requirements-register.md` | `specs/system/04-requirements/req-quality-report.md` |
| `se-plan` | `specs/system/01-conops/`, `specs/system/02-nfr/`, `specs/features/$FEATURE_ID/03-explore/`, `specs/system/04-requirements/` | `specs/features/$FEATURE_ID/07-plan/plan.md` |
| `se-architecture` | `specs/features/$FEATURE_ID/07-plan/plan.md`, `specs/system/02-nfr/`, `specs/system/04-requirements/` | `specs/system/05-architecture/` |
| `se-interfaces` | `specs/system/05-architecture/`, `specs/system/04-requirements/` | `specs/system/06-interfaces/icd.md` |
| `se-vvplan` | `specs/system/04-requirements/`, `specs/system/02-nfr/`, `specs/system/06-interfaces/icd.md` | `specs/system/09-vv/vv-plan.md` (finalised), `specs/system/09-vv/vv-results.md` |
| `se-rtm` | `specs/system/` (most artifacts), `specs/features/$FEATURE_ID/08-tasks/tasks.md` | `specs/system/rtm.md` (finalised), `specs/system/traceability-report.md` |
| `se-risk` | `specs/system/10-risk/risk-register.md`, `specs/system/09-vv/vv-results.md` | `specs/system/10-risk/risk-register.md` (finalised), `specs/system/10-risk/risk-closure-report.md` |
| `se-gate` | all artifacts for completed phase | `specs/features/$FEATURE_ID/gate-reports/gate-{N}-{pass\|fail}.md` |

## Design Principles

**Everything as Code.** All SE artifacts are plain text (Markdown) or text-based diagrams (Mermaid) committed to the repository. They are version-controlled, diff-able, and reviewable in pull requests.

**Threaded artifacts are never skipped.** The risk register, V&V plan, and RTM are not end-of-project reports. They are initiated early and updated as an obligation of each subsequent skill.

**Phase gates enforce completeness.** No phase is complete until `/se-gate N` passes. Gates check threaded artifact currency, not just sequenced artifact existence.

**Set-Based Design over premature convergence.** The `/se-explore` skill preserves multiple solution options in parallel. Options are eliminated only when evidence — not opinion — rules them out.  Elimination rationale is always recorded.

**Verifiability is a requirement quality attribute.** Every requirement must have a corresponding V&V plan entry before Phase 2 closes. A requirement without a verification method is incomplete by  definition.
