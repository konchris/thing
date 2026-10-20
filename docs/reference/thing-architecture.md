# THING Architecture and Workflow Reference

This document is the authoritative reference for two audiences:

- **Agents maintaining THING** — understanding what THING is, how it
  integrates with spec-kit, and the design intent behind its commands
  and hooks
- **Agents and users running THING** — choosing the right workflow path
  for a given project and understanding which commands supersede spec-kit
  and which augment it

---

## What THING Is

THING is a standalone SE workflow library for AI coding agents. It runs
inside an AI coding agent (Claude Code or equivalent) when invoked via
slash commands. It is not a spec-kit extension, not a wrapper around
spec-kit, and not a Claude chat skill.

THING installs as a git submodule. Its commands are invoked directly by
the agent — by reading the command files in `commands/` — or via the
`/thing.{name}` slash command namespace when the optional spec-kit
integration shim (`extension.yml`) is registered.

All THING artifacts are plain Markdown or Mermaid files committed to the
repository alongside the code they govern. THING has no runtime, no build
step, and no external dependencies.

THING does not replace spec-kit's code generation pipeline. It provides:

- The SE **front-end** (phases 0–1): problem space, stakeholder needs,
  ConOps, NFRs, Set-Based Design — entirely absent from spec-kit
- The **requirements and architecture middle** (phases 2–3): functional
  requirements derivation, IEEE 29148 quality gating, multi-view
  architecture, interface control — replaces spec-kit's lightweight
  specify/plan commands for SE features
- The **V&V and closure back-end** (phase 6): V&V plan finalisation, RTM
  closure, risk disposition — no spec-kit equivalent exists
- **Threaded artifact obligations**: risk register, V&V plan, and RTM
  accumulate evidence continuously across the lifecycle rather than being
  produced at the end as reports

---

## Two Operating Modes

THING operates in two distinct modes. The choice depends on what is already
known about the feature being built.

### Mode 1 — SE Feature Path

Use when the feature introduces new system behaviour, interfaces, or quality
obligations that do not yet have requirements. You do not yet know *what*
to build.

THING commands **supersede** the corresponding spec-kit commands. SE features
have no `spec.md`. The SE artifact set (ConOps, NFRs, requirements register,
plan) serves the role that `spec.md` serves in standard spec-kit features.

### Mode 2 — Standard Feature Path with THING Hooks

Use when requirements are established in the existing SE artifact set and the
feature is a known, bounded piece of implementation work. You know *what* to
build; you need to build it correctly.

THING commands **augment** spec-kit commands via hooks. The standard spec-kit
pipeline runs; THING hooks enforce quality gates and update threaded artifacts.

---

## Supersede vs. Augment — Explicit Contract

| Spec-Kit Command | SE Feature (Mode 1) | Standard Feature (Mode 2) |
| --- | --- | --- |
| `speckit.constitution` | Runs first. `thing.se-constitution` hook enriches it. | Same. |
| `speckit.specify` | **Superseded.** Run `thing.se-requirements` instead. SE features have no `spec.md`. | Runs unchanged. `thing.se-req-quality` augments via `after_specify` hook. |
| `speckit.clarify` | Optional — may run after `thing.se-requirements` if areas remain underspecified. | Unchanged. |
| `speckit.checklist` | Optional — may run after `thing.se-requirements`. | Unchanged. |
| `speckit.plan` | **Superseded.** Run `thing.se-plan` then `thing.se-architecture` instead. | Runs unchanged. `thing.se-architecture` augments via `after_plan` hook. |
| `speckit.tasks` | **Superseded.** Run `thing.se-tasks` instead. Reads requirements register, not `spec.md`. | Runs unchanged. `thing.se-rtm` augments via `after_tasks` hook. |
| `speckit.analyze` | **Superseded.** Run `thing.se-analyze` instead. Reads SE artifact set, not `spec.md`. | Runs unchanged. `thing.se-gate` augments via `after_analysis` hook. |
| `speckit.implement` | **Unchanged.** Runs as-is. `thing.se-rtm` hook updates RTM. | Unchanged. Same hook applies. |
| `speckit.taskstoissues` | Optional — available in both modes. | Unchanged. |

**Rule for agents:** If you are working an SE feature and find yourself about
to run `speckit.specify`, `speckit.plan`, `speckit.tasks`, or
`speckit.analyze`, stop. Run the corresponding THING command instead. These
spec-kit commands will fail gracefully on SE features (no `spec.md` exists).
The correct response is to use the THING path, not to create a `spec.md`.

---

## Hook Obligation Defaults

| Hook writes to | optional | Rationale |
| --- | --- | --- |
| Threaded artifact (RTM, risk register, V&V plan) | `false` | Obligation — skipping breaks traceability |
| New sequenced artifact | `true` | Agent may legitimately skip on simple features |

Threaded artifact hooks: `after_specify` → `thing.se-req-quality`,
`after_tasks` → `thing.se-rtm`, `after_implement` → `thing.se-rtm`.

Sequenced artifact hooks: `after_constitution` → `thing.se-constitution`,
`after_plan` → `thing.se-architecture`, `after_analysis` → `thing.se-gate`.

---

## Workflow Paths

Four paths are available. The correct path depends on two questions:

1. Do system-level SE artifacts already exist for this system?
2. Does this feature introduce new interfaces or quality obligations not
   covered by the existing artifact set?

```text
START
  │
  ├─ No system-level SE artifacts exist yet
  │   └──► PATH A — Full SE (phases 0–6)
  │
  ├─ System artifacts exist; feature introduces novel requirements
  │  or new interfaces not in the current ICD or NFR set
  │   └──► PATH B — SE from Phase 2
  │
  ├─ System artifacts exist; requirements fully defined;
  │  feature is a bounded implementation of a known component
  │   └──► PATH C — Standard spec-kit + THING hooks
  │
  └─ Simple internal feature; no system boundary crossings;
     no new quality obligations
      └──► PATH D — Standard spec-kit only
```

### Path A — Full SE

*New system. Nothing is yet known about requirements, architecture,
or quality obligations.*

```text
Phase 0 — Problem Space
  speckit.constitution
    └─(hook)─ thing.se-constitution       [optional: true]
  thing.se-stakeholders
  thing.se-conops
  THREADS INITIATED: risk-register

Phase 1 — Solution Space
  thing.se-nfr
  thing.se-explore
  thing.se-gate 1
  THREADS INITIATED: vv-plan

Phase 2 — Requirements
  thing.se-requirements
  thing.se-req-quality
  thing.se-gate 2
  THREADS INITIATED: rtm

Phase 3 — Architecture
  thing.se-plan
  thing.se-architecture
  thing.se-interfaces
  thing.se-gate 3

Phase 4 — Implementation Planning
  thing.se-tasks
  thing.se-analyze
  thing.se-gate 4

Phase 5 — Implementation
  speckit.implement
    └─(hook)─ thing.se-rtm               [optional: false]
  thing.se-gate 5

Phase 6 — V&V and Closure
  thing.se-vvplan
  thing.se-rtm (finalise)
  thing.se-risk
  thing.se-gate 6
```

Use when: new system with no existing SE artifacts, or any regulated
system where a full audit trail is required from first principles.

### Path B — SE from Phase 2

*System-level artifacts exist from a prior Path A run. This feature
introduces new requirements or new interfaces not yet in the artifact set.*

The system-level artifacts are already present. Skip phases 0 and 1.
Begin from `thing.se-requirements`, which reads the existing ConOps and
NFRs. Extend the ICD with new interface entries if needed. Gate checks
confirm threaded artifacts are current before allowing progression.

```text
[Existing: stakeholder register, ConOps, NFRs, architecture, ICD, RTM,
risk register, V&V plan]

Phase 2 — Requirements
  thing.se-requirements       ← reads existing ConOps, NFRs
  thing.se-req-quality
  thing.se-gate 2

Phase 3 — Architecture
  thing.se-plan
  thing.se-architecture       ← extends existing architecture docs
  thing.se-interfaces         ← adds new entries to existing ICD
  thing.se-gate 3

Phase 4 — Implementation Planning
  thing.se-tasks
  thing.se-analyze
  thing.se-gate 4

Phase 5 — Implementation
  speckit.implement
    └─(hook)─ thing.se-rtm
  thing.se-gate 5

Phase 6 — V&V and Closure
  thing.se-vvplan
  thing.se-rtm (finalise)
  thing.se-risk
  thing.se-gate 6
```

Use when: iterative development on an established system where new features
introduce novel requirements or interface obligations.

### Path C — Standard spec-kit + THING Hooks

*System artifacts exist. Requirements are established. Feature is a bounded
implementation of known, documented requirements.*

```text
speckit.constitution (if new project)
speckit.specify
  └─(hook)─ thing.se-req-quality    [optional: false — updates risk register]
speckit.clarify (if needed)
speckit.plan
  └─(hook)─ thing.se-architecture   [optional: true]
speckit.tasks
  └─(hook)─ thing.se-rtm            [optional: false — updates RTM Task Ref]
speckit.implement
  └─(hook)─ thing.se-rtm            [optional: false — updates RTM impl refs]
```

Use when: well-understood features implementing known requirements, where
the value is RTM currency and quality gating rather than upstream exploration.

### Path D — Standard spec-kit Only

No THING involvement. Use for internal refactors, tooling changes, or
features with no system boundary crossings and no regulated quality
obligations.

---

## System-Level vs. Feature-Scoped Artifacts

**System-level artifacts** (`specs/system/`) describe the system as a whole.
Created once, updated in place with each feature iteration. The RTM gains new
rows. The risk register gains new entries. Architecture views are extended,
not replaced.

Each system-level artifact is a single current baseline. A later feature
merges into the existing tables instead of appending a section of its own:
changed rows are edited in place, rows that are no longer true are struck
through and retired (never deleted, IDs never reused), and prose the feature
has made untrue is rewritten. The header carries
`**Baseline as of:** {feature ID}, {date}`.

What a feature changed in the baseline, and why, is recorded in a feature
delta, `specs/features/{feature-id}/{NN-phase}/{name}-delta.md` (template:
`artifacts/baseline-delta.md`). A skill checks only the system artifacts of
its own and earlier development steps. Phase gate N runs the Baseline Currency
Checks over the artifacts of phases 0 to N: deltas exist, each artifact is
still one document, and no statement is contradicted by the feature.

**Feature-scoped artifacts** (`specs/features/{feature-id}/`) capture the
exploration and planning work for a single feature. Created fresh per feature.
Not updated after the feature closes — they are a historical record.

| Artifact | Scope | Lifecycle |
| --- | --- | --- |
| Stakeholder register, needs register, personas, empathy maps | System | Updated if new stakeholders emerge |
| ConOps, operational scenarios | System | Updated if operational picture changes |
| Quality attributes (NFRs) | System | Updated if new NFRs introduced |
| Requirements register, quality report | System | Extended each feature |
| Architecture docs, views, ADRs | System | Extended each feature; ADRs immutable once accepted |
| Interface Control Document | System | Extended each feature |
| V&V plan, V&V results | System | Extended each feature |
| Risk register | System | Continuously updated; entries never deleted |
| RTM | System | Continuously updated; rows never deleted |
| Explore summary, option files, elimination record | Feature | Historical record; closed at feature end |
| Plan | Feature | Historical record; closed at feature end |
| Tasks | Feature | Historical record; closed at feature end |
| Gate reports | Feature | Audit trail; closed at feature end |
| Baseline deltas (`*-delta.md`) | Feature | What the feature changed in each system artifact and why; closed at feature end |

---

## Inter-Command Input/Output Contract

| Command | Reads | Writes |
| --- | --- | --- |
| `thing.se-constitution` | user input | `constitution.md`, `specs/system/10-risk/risk-register.md` |
| `thing.se-stakeholders` | `constitution.md` | `specs/system/00-stakeholders/`, `specs/system/10-risk/risk-register.md` |
| `thing.se-conops` | `specs/system/00-stakeholders/`, `constitution.md` | `specs/system/01-conops/` |
| `thing.se-nfr` | `specs/system/01-conops/`, `specs/system/00-stakeholders/needs-register.md` | `specs/system/02-nfr/quality-attributes.md`, `specs/system/09-vv/vv-plan.md` |
| `thing.se-explore` | `specs/system/00-stakeholders/needs-register.md`, `specs/system/02-nfr/quality-attributes.md` | `specs/features/$FEATURE_ID/03-explore/` |
| `thing.se-requirements` | `specs/system/01-conops/`, `specs/system/02-nfr/`, `specs/features/$FEATURE_ID/03-explore/explore-summary.md` | `specs/system/04-requirements/requirements-register.md`, `specs/system/09-vv/vv-plan.md`, `specs/system/rtm.md` |
| `thing.se-req-quality` | `specs/system/04-requirements/requirements-register.md` | `specs/system/04-requirements/req-quality-report.md` |
| `thing.se-plan` | `specs/system/01-conops/`, `specs/system/02-nfr/`, `specs/features/$FEATURE_ID/03-explore/`, `specs/system/04-requirements/` | `specs/features/$FEATURE_ID/07-plan/plan.md` |
| `thing.se-architecture` | `specs/features/$FEATURE_ID/07-plan/plan.md`, `specs/system/02-nfr/`, `specs/system/04-requirements/` | `specs/system/05-architecture/` |
| `thing.se-interfaces` | `specs/system/05-architecture/`, `specs/system/04-requirements/` | `specs/system/06-interfaces/icd.md` |
| `thing.se-tasks` | `specs/system/04-requirements/requirements-register.md`, `specs/features/$FEATURE_ID/07-plan/plan.md`, `specs/system/05-architecture/architecture.md` | `specs/features/$FEATURE_ID/08-tasks/tasks.md`, `specs/system/rtm.md` |
| `thing.se-analyze` | `specs/system/04-requirements/requirements-register.md`, `specs/features/$FEATURE_ID/07-plan/plan.md`, `specs/features/$FEATURE_ID/08-tasks/tasks.md`, `specs/system/rtm.md` | analysis report (in-conversation only) |
| `thing.se-vvplan` | `specs/system/04-requirements/`, `specs/system/02-nfr/`, `specs/system/06-interfaces/icd.md` | `specs/system/09-vv/vv-plan.md`, `specs/system/09-vv/vv-results.md` |
| `thing.se-rtm` | most `specs/system/` artifacts, `specs/features/$FEATURE_ID/08-tasks/tasks.md` | `specs/system/rtm.md`, `specs/system/traceability-report.md` |
| `thing.se-risk` | `specs/system/10-risk/risk-register.md`, `specs/system/09-vv/vv-results.md` | `specs/system/10-risk/risk-register.md`, `specs/system/10-risk/risk-closure-report.md` |
| `thing.se-gate` | all artifacts for completed phase | `specs/features/$FEATURE_ID/gate-reports/gate-{N}-{pass\|fail}.md` |

---

## Maintenance Notes

When modifying THING commands or adding new ones:

1. **Commands are self-contained.** An agent executing a command reads only
   that file plus the artifacts listed in its pre-conditions. Do not assume
   the agent has read other command files.

2. **Threaded artifact updates are obligations.** Never remove the threaded
   artifact update steps from any command.

3. **Gates check currency, not existence.** The `thing.se-gate` command checks
   that threaded artifacts have been updated since the previous gate. Ensure
   `Last Updated By` and date fields are written correctly by every command
   that updates a threaded artifact.

4. **The supersede relationship is total.** Never add fallback logic that reads
   `spec.md` when SE artifacts are absent. Missing pre-condition → stop and
   name the command that produces it.
