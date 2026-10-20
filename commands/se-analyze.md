---
description: "Perform a non-destructive cross-artifact consistency and quality analysis across the SE Phase 4 artifact set: requirements register, plan, tasks, and RTM. Identifies coverage gaps, ambiguities, terminology drift, and constitution violations before implementation begins. This skill replaces /speckit.analyze for SE features, which have no spec.md."
---

# SE Analyze

## Purpose

Identify inconsistencies, coverage gaps, ambiguities, and
underspecified items across the SE Phase 4 artifact set before
implementation begins. This skill does the job that
`/speckit.analyze` does for standard Spec-Kit features, but
reads SE artifacts (requirements register, plan, tasks, RTM)
instead of `spec.md`.

**This skill is strictly read-only.** It does not modify any
files. It produces a structured analysis report in-conversation
and offers remediation suggestions, but applies nothing without
explicit user approval.

This skill should be run after `/thing.se-tasks` and
before `/thing.se-gate 4`.

## Pre-Conditions

- `specs/system/04-requirements/requirements-register.md`
  must exist. This is the SE equivalent of spec.md — it is
  the primary source of truth.
- `specs/features/$FEATURE_ID/07-plan/plan.md` must exist.
- `specs/features/$FEATURE_ID/08-tasks/tasks.md` must exist.
  If it does not exist, stop and tell the user to run
  `/thing.se-tasks` first.
- `specs/system/rtm.md` must exist.
- `.specify/memory/constitution.md` must exist. Read it.
  Constitution principles are non-negotiable: any conflict
  is automatically CRITICAL.

Optional (read if present):

- `specs/system/05-architecture/architecture.md`
- `specs/system/02-nfr/quality-attributes.md`
- `specs/system/00-stakeholders/needs-register.md`

## Severity Levels

- **CRITICAL** — Violates a constitution MUST, requirement with
  zero task coverage that would block baseline functionality,
  missing mandatory artifact
- **HIGH** — Conflicting requirements, ambiguous measurable
  criterion, task references a non-existent component or file
  path pattern, RTM row with no Task Ref that is not a known gap
- **MEDIUM** — Terminology drift across artifacts, plan component
  not reflected in tasks, underspecified edge case, missing error
  handling path
- **LOW** — Style or wording improvements, minor redundancy,
  formatting inconsistency

## Instructions

### Step 1 — Read and Internalise Context

Read all pre-condition artifacts. Build internal models of:

**Requirements inventory** — for each requirement in the register:

- Req ID (FR-NNN or NFR-NNN)
- Full requirement text
- Priority (Must Have / Should Have / Nice to Have)
- Traced need (NEED-ID)
- Quality Status (PASS / FAIL / PASS WITH WARNINGS)

**Plan model** — from plan.md:

- Component decomposition (component names, purposes)
- Requirements Coverage Check table (which requirements
  are addressed by which components)
- Implementation phase structure

**Task model** — from tasks.md:

- All task IDs, their `[FR-NNN]` labels, and descriptions
- Phase grouping
- Parallelisable task markers `[P]`
- Referenced file paths or component names

**RTM model** — from rtm.md:

- Task Ref column state for each requirement
- Coverage metrics already recorded

**Constitution rules** — from constitution.md:

- All MUST and SHOULD normative statements
- Technology constraints and exclusions

### Step 2 — Check Extension Hooks

Check if `.specify/extensions.yml` exists. If it does, look for
`hooks.before_analyze` entries and process them using standard
hook logic (filter disabled, respect optional flag).

### Step 3 — Run Detection Passes

Work through each detection pass systematically. Limit total
findings to 50; if more exist, aggregate the remainder in an
overflow summary.

#### Pass A — Coverage Gaps

*For each requirement in the register:*

- Is there at least one task in tasks.md with a `[FR-NNN]`
  label matching this requirement's ID?
- If not, is the requirement addressed by a task without a
  label (infer by keyword match)?
- Flag any requirement with zero task coverage as a gap.

*For each task in tasks.md with a `[FR-NNN]` label:*

- Does that FR-NNN exist in the requirements register?
- Flag any task referencing a non-existent requirement ID
  as an orphaned task.

*For each requirement in the RTM with Task Ref = TBD or blank:*

- Cross-reference against tasks.md
- If tasks exist but RTM is not updated, flag as an RTM
  inconsistency (MEDIUM)

#### Pass B — Ambiguity Detection

*In the requirements register:*

- Flag vague adjectives without measurable criteria:
  "fast", "reliable", "secure", "user-friendly", "appropriate",
  "robust", "flexible", "scalable", "intuitive", "adequate"
- Flag undefined pronouns: "it", "they", "the system" where
  multiple systems are in scope
- Flag unresolved placeholders: TBD, TBC, TODO, ???

*In tasks.md:*

- Flag task descriptions without a specific file path or
  component name (tasks must be specific enough to execute
  without additional context)

#### Pass C — Inconsistency Detection

*Terminology drift:*

- Identify the same concept named differently across
  requirements-register.md, plan.md, and tasks.md
  (e.g., "authentication service" vs "auth module" vs
  "login handler" when referring to the same component)
- Flag each distinct terminology discrepancy

*Plan vs tasks misalignment:*

- Components named in plan.md that have no tasks in tasks.md
- Implementation phases defined in plan.md that have no
  corresponding task phase

*Task ordering:*

- Tasks marked as parallelisable `[P]` that have an
  unresolved dependency on an incomplete task in the
  same phase

#### Pass D — Constitution Alignment

For each MUST statement in the constitution:

- Is there a requirement, plan decision, or task that
  directly addresses or respects this constraint?
- Does any requirement, plan choice, or task contradict it?
- Flag any violation as CRITICAL

#### Pass E — Completeness

*Requirements:*

- Any requirement with Quality Status = FAIL in the register
  that has not been noted as resolved (from se-req-quality)
- Any requirement with no traced need (NEED-ID blank or TBD)
- Must Have requirements with no task coverage

*Plan:*

- Any "Open Technical Question" (OTQ) in plan.md that is still
  marked unresolved — flag as HIGH if it affects a Must Have
  requirement

*Tasks:*

- Any task in the Polish phase that is not clearly traceable
  to a specific requirement or quality concern (vague polish
  tasks are LOW)

#### Pass F — RTM Consistency

- Count requirements in the register vs. rows in the RTM —
  they must match
- Count requirements in the RTM with Task Ref populated vs.
  total — if coverage < 100% and it cannot be explained by
  known gaps, flag as MEDIUM
- Verify the RTM Coverage section statistics match what
  tasks.md actually contains

### Step 4 — Severity Assignment

For each finding, assign a severity using the levels defined above.
Be precise about the location: cite the Req ID, task ID, or section
heading where the issue occurs.

### Step 5 — Produce Analysis Report

Output the following report directly in the conversation.
Do not write any file.

```markdown
## SE Artifact Analysis Report

**Feature:** {feature ID and name}
**Date:** {date}
**Artifacts analysed:** requirements-register.md, plan.md,
  tasks.md, rtm.md, constitution.md

---

### Findings

| ID | Category | Severity | Location | Summary | Recommendation |
| ---- | ---------- | ---------- | ---------- | --------- | ---------------- |
| A1 | Coverage Gap | CRITICAL | FR-007 | No tasks reference FR-007 | Add tasks implementing {requirement text} |
| B1 | Ambiguity | MEDIUM | requirements-register.md FR-003 | "fast response" lacks measurable criterion | Define response time bound (e.g., ≤ 200ms) |

---

### Coverage Summary

| Req ID | Priority | Has Tasks? | Task IDs | Notes |
| -------- | ---------- | ------------ | ---------- | ------- |
| FR-001 | Must Have | Yes | T004, T006 | |
| FR-007 | Must Have | No | — | CRITICAL gap |

---

### Constitution Alignment Issues

{List any constitution violations, or "None identified."}

---

### Orphaned Tasks

{Tasks with a [FR-NNN] label referencing a non-existent
requirement, or "None identified."}

---

### RTM Consistency

{Summary of RTM vs tasks.md consistency, or "RTM consistent
with tasks.md."}

---

### Metrics

- Total requirements: {N}
- Requirements with task coverage: {N} ({%})
- Total tasks: {N}
- Tasks with requirement labels: {N}
- Orphaned tasks: {N}
- CRITICAL findings: {N}
- HIGH findings: {N}
- MEDIUM findings: {N}
- LOW findings: {N}

---

### Overflow Summary

{If more than 50 findings were identified, summarise the
remainder here by category and count.}
```

### Step 6 — Next Actions

After the report, output a concise next actions block:

- If CRITICAL issues exist: state that these must be resolved
  before running `/thing.se-gate 4`. Name the specific
  issue and which artifact to fix.
- If only MEDIUM or LOW issues: state that the user may proceed
  to the gate, but list the improvements recommended before
  implementation begins.
- Always end with: *"Run `/thing.se-gate 4` when all
  CRITICAL issues are resolved."*

### Step 7 — Offer Remediation

Ask: *"Would you like me to suggest concrete remediation edits
for the top issues? I will not apply any changes automatically —
you would need to run the relevant skill or approve edits
explicitly."*

### Step 8 — Check After-Analyze Extension Hooks

Check `.specify/extensions.yml` for `hooks.after_analyze`
entries and process them using standard hook logic.
