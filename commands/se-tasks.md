---
description: "Generate an implementation task list for an SE feature from the requirements register and plan. Organises tasks by requirement (FR-NNN) rather than user stories. Writes to 08-tasks/tasks.md and updates the RTM Task Ref column. This skill replaces /speckit.tasks for SE features, which have no spec.md."
handoffs:
  - label: Analyze for Consistency
    agent: thing.se-analyze
    prompt: Run SE artifact consistency analysis before the Phase 4 gate
    send: true
  - label: Run Phase 4 Gate
    agent: thing.se-gate
    prompt: "4"
    send: true
---

# SE Tasks

## Purpose

Decompose the requirements register and feature plan into a concrete,
dependency-ordered implementation task list. This skill does the job
that `/speckit.tasks` does for standard Spec-Kit features, but reads
SE artifacts (requirements register, plan, architecture) instead of
`spec.md`.

Tasks are organised by requirement — each functional requirement (FR-NNN)
gets the set of implementation tasks needed to satisfy it. This preserves
bidirectional traceability: every task links to a requirement, and the RTM
Task Ref column is updated so every requirement links to its tasks.

Output artifacts:

- `specs/features/$FEATURE_ID/08-tasks/tasks.md`

Updates:

- `specs/system/rtm.md` (Task Ref column for each requirement)
- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it.
- `specs/system/04-requirements/requirements-register.md`
  must exist. Read it. This is the primary input — the
  full requirement set with IDs, priorities, and traces.
- `specs/features/$FEATURE_ID/07-plan/plan.md` must exist.
  Read it. The implementation sequence and phase breakdown
  from the plan drives task ordering.
- `specs/system/05-architecture/architecture.md` must exist.
  Read it. The component decomposition identifies which
  components each requirement is allocated to — tasks are
  written against specific components and file paths.
- `specs/system/rtm.md` must exist. Read it.
  The Task Ref column will be updated by this skill.

If any pre-condition is not met, stop and tell the user
which skill must be run first:

- Missing requirements register → `/thing.se-requirements`
- Missing plan → `/thing.se-plan`
- Missing architecture → `/thing.se-architecture`
- Missing RTM → `/thing.se-requirements`

## Task Format

Every task **must** follow this checklist format:

```text
- [ ] T001 [P] [FR-003] Description with specific file path or component
```

**Format components:**

1. **Checkbox** — always `- [ ]`
2. **Task ID** — sequential `T001`, `T002`, … in execution order
3. **`[P]` marker** — include only if the task is parallelisable
   (operates on a different file or component from adjacent tasks,
   no unresolved dependency on an incomplete task)
4. **`[FR-NNN]` label** — the requirement this task implements.
   - Setup phase tasks: no label
   - Foundational phase tasks: no label
   - Requirement-phase tasks: must have label
   - Polish phase tasks: no label
5. **Description** — a clear action with a specific file path or
   component name. Specific enough that an LLM can complete the
   task without additional context.

**Examples:**

```text
- [ ] T001 Create project structure and initialise {component} per plan.md
- [ ] T005 [P] [FR-003] Implement {capability} in src/{module}/{file}.py
- [ ] T012 [P] [FR-007] Add {interface method} to src/{component}/service.py
- [ ] T014 [FR-007] Wire {component} to {component} in src/{integration}.py
```

## Instructions

### Step 1 — Read and Internalise Context

Read all pre-condition artifacts. Build an internal model of:

- The full requirements set and their priorities (from requirements register)
- The phase breakdown and component decomposition (from plan)
- Which components satisfy which requirements (from plan's
  Requirements Coverage Check section and architecture)
- Current RTM state — which requirements still have TBD Task Ref

### Step 2 — Check Extension Hooks

Check if `.specify/extensions.yml` exists in the project root.
If it exists, look for entries under the `hooks.before_tasks` key.
If found, handle them using the same hook processing logic as
`speckit.tasks` — filter disabled hooks, respect the `optional`
flag, check conditions.

### Step 3 — Derive the Phase Structure

Use the Implementation Sequence from `07-plan/plan.md` to define
the phase structure for tasks. The plan's Phase Breakdown section
already identifies which requirements belong to which phase.

Map plan phases to task phases:

- **Phase 1 — Setup:** infrastructure, scaffolding, shared
  tooling. No requirement labels.
- **Phase 2 — Foundation:** blocking prerequisites that all
  requirement phases depend on. No requirement labels.
- **Phase 3 … N — Requirement Phases:** one phase per
  functional requirement (or group of closely related
  requirements that share the same component). Order by:
  1. Plan implementation phase (Phase 1 Must Haves first)
  2. Within a plan phase, Must Have before Should Have
  3. Within a priority tier, foundational requirements
     before dependent ones
- **Final Phase — Polish:** cross-cutting concerns, error
  handling completeness, logging, documentation. No requirement
  labels.

### Step 4 — Decompose Requirements into Tasks

For each functional requirement in the register, working through
the phase structure derived in Step 3:

1. Identify the component or module responsible for satisfying
   this requirement (from the plan's component decomposition or
   the architecture's container/component views)
2. Break the requirement down into concrete implementation steps.
   Each step must be:
   - A single file change or a single testable action
   - Specific enough to execute without ambiguity
   - Associated with a file path or named component
3. Identify which tasks within this requirement are parallelisable
4. Assign task IDs sequentially across the entire document (T001,
   T002, … — not per phase)
5. Apply the `[FR-NNN]` label to every task in this requirement's
   phase

For NFRs that require functional mechanisms (e.g., a health check
endpoint driven by an availability NFR), decompose those mechanisms
into tasks and label them with the NFR ID.

**Typical decomposition pattern for a functional requirement:**

```text
- [ ] T0NN [FR-NNN] Define {data structure or interface contract}
- [ ] T0NN [P] [FR-NNN] Implement {primary capability} in {file}
- [ ] T0NN [P] [FR-NNN] Implement {secondary capability} in {file}
- [ ] T0NN [FR-NNN] Integrate {component A} with {component B}
```

Do not decompose to the level of individual lines of code. Each
task should represent approximately 30–120 minutes of focused
implementation work.

### Step 5 — Generate the Dependency Graph

After all tasks are defined, document dependencies:

- Which phases must complete before the next phase can begin
- Within a phase, which tasks must complete before others can start
- Tasks marked `[P]` within a phase can run in parallel

### Step 6 — Write Tasks File

Create directory `specs/features/$FEATURE_ID/08-tasks/` if it
does not exist.

Write `specs/features/$FEATURE_ID/08-tasks/tasks.md`:

````markdown
# Implementation Tasks

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** READY
**Last Updated By:** se-tasks skill

---

## Overview

**Total tasks:** {N}
**Parallelisable tasks:** {N}
**Phases:** {N}
**Estimated implementation phases from plan:** {list}

---

## Phase 1 — Setup

**Goal:** Initialise project structure and shared infrastructure.
**Entry criterion:** Architecture approved, Phase 3 gate passed.
**Exit criterion:** All setup tasks complete; foundation phase can begin.

- [ ] T001 {task description}
- [ ] T002 {task description}

---

## Phase 2 — Foundation

**Goal:** Implement blocking prerequisites for all requirement phases.
**Entry criterion:** Phase 1 complete.
**Exit criterion:** Foundation complete; requirement phases can begin in parallel.

- [ ] T003 {task description}

---

## Phase 3 — {Requirement Group or FR Title}

**Requirements addressed:** FR-001, FR-002
**Goal:** {what this phase delivers — one sentence}
**Entry criterion:** Phase 2 complete.
**Exit criterion:** All tasks in this phase complete and independently testable.

- [ ] T004 [P] [FR-001] {description} in {file path}
- [ ] T005 [P] [FR-002] {description} in {file path}
- [ ] T006 [FR-001] {integration task}

---

## Phase {N} — Polish and Cross-Cutting Concerns

**Goal:** Error handling completeness, observability, documentation.
**Entry criterion:** All requirement phases complete.
**Exit criterion:** All tasks complete; system ready for V&V.

- [ ] T0NN {task description}

---

## Dependency Graph

```text
Phase 1 (Setup)
  └── Phase 2 (Foundation)
        ├── Phase 3 ({FR group}) [can run in parallel with Phase 4]
        ├── Phase 4 ({FR group}) [can run in parallel with Phase 3]
        └── ...
              └── Final Phase (Polish)
```

---

## Requirements Coverage

| Req ID | Priority | Tasks | Phase |
| ------ | -------- | ----- | ----- |
| FR-001 | Must Have | T004, T006 | 3 |
| FR-002 | Must Have | T005 | 3 |

Uncovered requirements: {list or "None — all requirements addressed"}

---

## Parallel Execution Groups

{For each phase that has parallelisable tasks, list the groups
that can run simultaneously.}

Phase 3 parallel group: T004, T005

````

### Step 7 — Update the RTM

Open `specs/system/rtm.md`.

For each requirement in the Matrix table, update the Task Ref
column with the Task IDs from this skill's output that implement
it (comma-separated if multiple).

Update the Coverage section:

```text
- Covered by task: {N} ({%}) — populated by se-tasks
```

Update Last Updated By to `se-tasks skill`.

### Step 8 — Update Risk Register

**Risk ID assignment:** Before appending new entries, scan
the register for all existing `RISK-NNN` IDs and determine
the maximum N. Assign new IDs starting from `max_N + 1`.
Never derive the next ID from the last row alone — use the
maximum across the entire document to prevent collisions.
Set `Status` = `Open` and `Residual P`, `Residual C`,
`Residual Level` = `—` for all newly created entries.
Update the Summary table counts after adding entries.

Add entries to `specs/system/10-risk/risk-register.md` for:

- Any functional requirement for which no concrete implementation
  task could be derived (requirement may be under-specified —
  flag for review before Phase 4 gate)
- Any requirement whose implementation tasks depend on an
  external system, vendor, or hardware not yet confirmed
- Any task group with no clear owner if the project team
  structure suggests this is a risk

### Step 9 — Check After-Tasks Extension Hooks

Check `.specify/extensions.yml` for `hooks.after_tasks` entries
and process them using the same logic as Step 2.

### Step 10 — Confirm Completion

Report to the user:

1. Total tasks generated
2. Tasks per phase
3. Number of parallelisable tasks
4. Requirements coverage: N of M requirements have at least one
   task (list any uncovered)
5. RTM Task Ref column coverage percentage
6. Number of risk entries added
7. Recommended next skill: `/thing.se-analyze`
