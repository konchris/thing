---
description: "Finalise the Requirements Traceability Matrix. The RTM has been built incrementally across all phases — this skill audits it for completeness, fills remaining gaps, calculates coverage metrics, and produces the final traceability report. A complete RTM proves every stakeholder need flows to a requirement, every requirement flows to a design element, every design element is covered by a task, and every task has a verification event."
---

# SE RTM

## Purpose

Finalise and certify the Requirements Traceability Matrix.
The RTM is the single document that proves the system is
complete: every stakeholder need is addressed, every
requirement is implemented, and every implementation is
verified.

The RTM has been built incrementally:

- Phase 2: requirement rows created by `/se-requirements`
- Phase 3: design element column populated by
  `/se-architecture` and `/se-interfaces`
- Phase 4: task reference column populated by
  `/speckit.tasks`
- Phase 5: implementation reference added by
  `/speckit.implement`
- Phase 6: V&V references added by `/se-vvplan`

This skill audits every column of every row, identifies
and resolves gaps, calculates coverage metrics, and
produces a final traceability report that can be
submitted to a customer, regulator, or auditor.

**Complete traceability means:**

- Every stakeholder need → at least one requirement
- Every requirement → at least one design element
- Every requirement → at least one task
- Every requirement → at least one verification event
- Every verification event → a result (pass, fail, or waived)

Output artifacts:

- `specs/system/rtm.md` (finalised)
- `specs/system/traceability-report.md`
- `specs/features/$FEATURE_ID/rtm-delta.md` (feature delta)

Updates:

- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `specs/system/rtm.md` must exist.
- `specs/system/04-requirements/requirements-register.md`
  must exist. Read it.
- `specs/system/00-stakeholders/needs-register.md`
  must exist. Read it.
- `specs/system/05-architecture/architecture.md`
  must exist. Read it.
- `specs/system/06-interfaces/icd.md` must exist.
  Read it.
- `specs/features/$FEATURE_ID/08-tasks/tasks.md` must exist.
  Read it.
- `specs/system/09-vv/vv-plan.md` must exist.
  Read it.
- `specs/system/09-vv/vv-results.md` must exist.
  Read it.

If any pre-condition is not met, stop and tell the user
which artifact is missing and which skill produces it.

## Traceability Chain

The RTM enforces four traceability chains. Each chain
must be complete for the RTM to pass.

### Chain 1 — Forward Traceability (Need to Verification)

```text
Stakeholder Need (NEED-ID)
  → Requirement (REQ-ID)
    → Design Element (container, component, or IF-ID)
      → Task (TASK-ID)
        → Verification Event (VVP-ID)
          → Result (Pass | Fail | Waived)
```

### Chain 2 — Backward Traceability (Verification to Need)

```text
Verification Event (VVP-ID)
  → Requirement (REQ-ID)
    → Stakeholder Need (NEED-ID)
```

Every verification event must trace back to a need.
A verification event with no traceable need is verifying
gold-plating.

### Chain 3 — NFR Traceability

```text
NFR (NFR-ID)
  → Architectural Mechanism (ADR-ID or design element)
    → Verification Event (VVP-ID)
      → Result
```

NFRs trace differently from functional requirements —
they are typically satisfied by architectural decisions
rather than individual components.

### Chain 4 — Interface Traceability

```text
Interface (IF-ID)
  → Interface Requirement (REQ-ID or IF-ID direct)
    → Verification Event (VVP-ID)
      → Result
```

## Baseline-Update Mode

The system-level artifacts this skill writes are a baseline: one
current description of the whole system, not a log of features.
This applies to:

- `specs/system/rtm.md`

and to every edit this skill makes to a threaded artifact (risk
register, V&V plan, RTM).

Before writing, check whether each artifact already exists.

- **Does not exist** — this is the first feature to touch it.
  Write it from the template in the Instructions below.
- **Exists** — merge into it. Never add a section for this feature
  (no "§7 Feature 011", no "Feature 012 additions") and never
  write a second copy of a table that already exists.

When merging:

1. Read the existing artifact in full before changing anything.
2. If the artifact still has per-feature sections or split tables
   from before this convention (e.g. "§7 Feature 011", a second
   modes table, "Section 6 — … (Feature 009)"), consolidate them
   as part of this merge: move each row into the one table for
   its concept, move prose into the section it belongs to, keep
   every ID, and remove the feature headings. Do not change
   content while moving it, except to correct statements that are
   no longer true. Record each fold in the feature delta as
   `Consolidated`.
3. Requirements live in one matrix for the whole system. Never
   delete an RTM row.
4. Add new rows to the existing table. Edit changed rows in place;
   where the history matters, add
   "revised {YYYY-MM-DD}, {feature ID}" to the row's notes.
5. Retire rows that are no longer true: strike through the text
   (`~~…~~`), set the Verification Status to `Retired`, and keep
   the row so its ID stays reserved. Record the reason in the
   feature delta, not in the baseline.
6. Rewrite prose so it describes the system as it is after this
   feature. A sentence this feature has made untrue is corrected,
   not left standing beside a newer one.
7. Keep IDs stable. Never renumber and never reuse an ID. New IDs
   start at the highest existing ID + 1.
8. Set the header to `**Baseline as of:** {feature ID}, {date}`.

Then list the services, interfaces, behaviours and constraints
this feature adds, removes or changes. Search the artifacts of
this and earlier development steps for statements about each
one:

- `specs/system/00-stakeholders/`
- `specs/system/01-conops/`
- `specs/system/02-nfr/`
- `specs/system/04-requirements/`
- `specs/system/05-architecture/`
- `specs/system/06-interfaces/`
- `specs/system/09-vv/`
- `specs/system/rtm.md`
- `specs/system/10-risk/risk-register.md` (threaded; this skill updates it)

Correct every statement in them that the feature has made
untrue, including in artifacts that earlier skills own.
Re-assess every risk, V&V entry or RTM row in the artifacts
above whose mitigation or subject the feature removes or
changes.

Finally, write the feature delta
`specs/features/$FEATURE_ID/rtm-delta.md` using the template at
`artifacts/baseline-delta.md`. If it already exists (this skill
ran earlier for the same feature), append to its tables. Record:

- **System Changes** — the list above
- **Baseline Edits** — one row per item or prose section added,
  changed, retired or consolidated, in any artifact listed above, with the
  reason
- **Statements Checked** — existing statements reviewed against
  the System Changes and left unchanged because they are still
  true

Write the delta on the first feature too; there every edit is
`Added`.

## Instructions

### Step 1 — Read All Artifacts

Read every pre-condition artifact in full. Build a
complete internal map of:

- All stakeholder needs and their IDs
- All requirements and their IDs
- All design elements from the architecture views
- All tasks from tasks.md
- All verification events from vv-plan.md
- All results from vv-results.md
- All interfaces from icd.md
- All NFRs from quality-attributes.md

### Step 2 — Audit Chain 1: Forward Traceability

For every requirement in the requirements register,
check each link in the forward chain:

#### Link 1: Need → Requirement

- Does this requirement have a source need ID?
- Does that need ID exist in the needs register?
- Flag: requirement with no need source as
  UNTRACED — requires justification or deletion

#### Link 2: Requirement → Design Element

- Does this requirement have a design element allocation?
- Does that element exist in the architecture views
  or ICD?
- Flag: requirement with no design allocation as
  UNALLOCATED

#### Link 3: Requirement → Task

- Is there at least one task in tasks.md that
  implements this requirement?
- Flag: requirement with no task as UNIMPLEMENTED

#### Link 4: Requirement → Verification Event

- Is there at least one entry in vv-plan.md for
  this requirement?
- Flag: requirement with no verification event as
  UNVERIFIED

#### Link 5: Verification Event → Result

- Does the verification event have a result in
  vv-results.md?
- Flag: verification event with no result as
  PENDING EXECUTION

### Step 3 — Audit Chain 2: Backward Traceability

For every verification event in vv-plan.md:

- Does it reference a requirement?
- Does that requirement reference a stakeholder need?
- Flag: verification event with no traceable need as
  ORPHANED VERIFICATION — may indicate gold-plating

For every requirement:

- Is it referenced by at least one need?
- If not: is there a documented rationale for why
  it exists without a direct need source?
  (Derived requirements are legitimate — they must
  have a parent requirement, not a need)

### Step 4 — Audit Chain 3: NFR Traceability

For every Must Have NFR in quality-attributes.md:

- Is there an ADR or design element that provides
  the architectural mechanism to satisfy it?
- Is there at least one verification event for it
  in vv-plan.md?
- Does that verification event have a result?
- Flag any Must Have NFR without all three as a
  gap requiring resolution

### Step 5 — Audit Chain 4: Interface Traceability

For every interface in icd.md:

- Is there at least one verification event in
  vv-plan.md that verifies this interface?
- Does that verification event have a result?
- Flag any interface without verification as
  UNVERIFIED INTERFACE

### Step 6 — Gap Resolution

For each gap identified in Steps 2–5, determine
the appropriate resolution:

**UNTRACED requirement** (no source need):
Ask the user: *"Requirement {REQ-ID} has no traceable
stakeholder need. Is this a derived requirement?
If so, which parent requirement does it derive from?
If it cannot be traced, it may need to be removed."*

**UNALLOCATED requirement** (no design element):
Ask the user: *"Requirement {REQ-ID} has not been
allocated to a design element. Which container,
component, or interface satisfies this requirement?"*

**UNIMPLEMENTED requirement** (no task):
Ask the user: *"Requirement {REQ-ID} has no
implementation task. Was this implemented as part
of another task? If so, which one? Or is it
outstanding?"*

**UNVERIFIED requirement** (no verification event):
Ask the user: *"Requirement {REQ-ID} has no
verification event. How will compliance be confirmed?
A verification event must be added to the V&V plan."*

**ORPHANED VERIFICATION** (no traceable need):
Ask the user: *"Verification event {VVP-ID} traces to
requirement {REQ-ID} which has no stakeholder need source.
Is this requirement necessary? If not, both the
requirement and its verification event may be candidates
for removal."*

### Step 7 — Calculate Coverage Metrics

Calculate the following metrics:

```textß
Need Coverage:
  Needs with at least one requirement / Total needs × 100%

Requirement Allocation Coverage:
  Requirements with design element / Total requirements × 100%

Requirement Implementation Coverage:
  Requirements with at least one task / Total requirements × 100%

Requirement Verification Coverage:
  Requirements with at least one V&V event / Total requirements × 100%

Verification Execution Coverage:
  V&V events with a result / Total V&V events × 100%

Verification Pass Rate:
  Passed V&V events / Total executed V&V events × 100%

NFR Coverage:
  Must Have NFRs with verification event / Total Must Have NFRs × 100%

Interface Verification Coverage:
  Interfaces with verification event / Total interfaces × 100%

Overall RTM Completeness:
  Average of all coverage metrics × 100%
```

The RTM is complete when all coverage metrics reach 100%
or every gap has documented justification.

### Step 8 — Write Finalised RTM

Rewrite `specs/system/rtm.md` as one matrix for the whole
system (see Baseline-Update Mode) with complete,
audited content:

```markdown
# Requirements Traceability Matrix

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** {DRAFT | COMPLETE | COMPLETE WITH EXCEPTIONS}
**Last Updated By:** se-rtm skill
**Version:** {increment from previous}

---

## Coverage Metrics

| Metric | Value | Target | Status |
| ------ | ----- | ------ | ------ |
| Need Coverage | {%} | 100% | {PASS/FAIL} |
| Requirement Allocation Coverage | {%} | 100% | {PASS/FAIL} |
| Requirement Implementation Coverage | {%} | 100% | {PASS/FAIL} |
| Requirement Verification Coverage | {%} | 100% | {PASS/FAIL} |
| Verification Execution Coverage | {%} | 100% | {PASS/FAIL} |
| Verification Pass Rate | {%} | 100% | {PASS/FAIL} |
| NFR Coverage | {%} | 100% | {PASS/FAIL} |
| Interface Verification Coverage | {%} | 100% | {PASS/FAIL} |
| **Overall RTM Completeness** | **{%}** | **100%** | **{PASS/FAIL}** |

---

## Traceability Matrix

| Req ID | Type | Requirement Title | Source Need | Design Element | Task Ref | V&V Ref | Result | Gaps |
| ------ | ---- | ----------------- | ----------- | -------------- | -------- | ------- | ------ | ---- |
| SRS-001 | Functional | {title} | NEED-STK-001-001 | Container-A | TASK-001 | VVP-001 | Pass | None |
| NFR-PERF-001 | NFR | {title} | NEED-STK-002-001 | ADR-002 | TASK-008 | VVP-012 | Pass | None |
| IF-001 | Interface | {title} | SRS-003 | IF-001 | TASK-005 | VVP-007 | Pass | None |

---

## Unresolved Gaps

{Populated only if gaps remain after Step 6 resolution.
Each gap requires documented justification.}

| Gap ID | Req ID | Gap Type | Justification | Owner | Resolution Target |
| ------ | ------ | -------- | ------------- | ----- | ----------------- |

---

## Exceptions

{Requirements or verification events that are accepted
as incomplete, with rationale and risk acceptance.}

| Exception ID | Req ID | Exception | Rationale | Risk Level | Accepted By |
| ------------ | ------ | --------- | --------- | ---------- | ----------- |
```

### Step 9 — Write Traceability Report

Write `specs/system/traceability-report.md` as
a standalone summary document suitable for submission
to a customer, regulator, or auditor:

```markdown
# Traceability Report

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Prepared By:** se-rtm skill
**Status:** {COMPLETE | COMPLETE WITH EXCEPTIONS}

---

## Executive Summary

{Two to three paragraphs. State the overall traceability
status, the coverage metrics achieved, and any exceptions
or gaps with their disposition. Written for a non-technical
audience — a programme manager, customer, or regulator
should be able to read this section and understand the
completeness status of the system.}

---

## Traceability Coverage

| Metric | Value | Status |
| ------ | ----- | ------ |

---

## Stakeholder Needs Coverage

{For each stakeholder in the register: how many of their
needs are addressed by requirements, and what is the
verification status of those requirements.}

| Stakeholder | Needs Total | Needs Covered | Verification Pass | Status |
| ----------- | ----------- | ------------- | ----------------- | ------ |

---

## Requirements Summary

| Category | Total | Allocated | Implemented | Verified | Passed |
| -------- | ----- | --------- | ----------- | -------- | ------ |
| Functional | | | | | |
| NFR — Must Have | | | | | |
| NFR — Should Have | | | | | |
| Interface | | | | | |
| **Total** | | | | | |

---

## Exceptions and Accepted Risks

{If none: state "No exceptions. All requirements are
fully traced, implemented, and verified."

If exceptions exist: list each one with its rationale
and the risk level accepted.}

---

## Conclusion

{One paragraph stating whether the system is ready
for acceptance based on traceability evidence.
State explicitly: "All stakeholder needs are addressed
by verified requirements" or list the specific
outstanding items that prevent this conclusion.}

---

## Referenced Artifacts

| Artifact | Location | Status |
| -------- | -------- | ------ |
| Stakeholder Register | `specs/system/00-stakeholders/stakeholder-register.md` | {status} |
| Needs Register | `specs/system/00-stakeholders/needs-register.md` | {status} |
| Requirements Register | `specs/system/04-requirements/requirements-register.md` | {status} |
| Quality Attributes | `specs/system/02-nfr/quality-attributes.md` | {status} |
| Architecture | `specs/system/05-architecture/architecture.md` | {status} |
| ICD | `specs/system/06-interfaces/icd.md` | {status} |
| V&V Plan | `specs/system/09-vv/vv-plan.md` | {status} |
| V&V Results | `specs/system/09-vv/vv-results.md` | {status} |
| RTM | `specs/system/rtm.md` | {status} |
| Risk Register | `specs/system/10-risk/risk-register.md` | {status} |
```

### Step 10 — Update Risk Register

Add entries for:

- Any unresolved gap that was accepted as an exception
  rather than resolved — record the accepted risk level
- Any requirement with a failed verification event
  that has not been resolved
- Any Must Have NFR with less than 100% verification
  coverage

### Step 11 — Confirm Completion

Report to the user:

1. Overall RTM completeness percentage
2. All coverage metrics with pass/fail status
3. Number of gaps found during audit
4. Number of gaps resolved vs. accepted as exceptions
5. Number of risk entries added
6. Whether the RTM status is COMPLETE or
   COMPLETE WITH EXCEPTIONS
7. Recommended next skill:
   `/thing.se-risk`
   (final risk register disposition before Phase 6 gate)

Also report the feature delta path, the number of baseline
items added, changed and retired, and every statement corrected
in an artifact this skill does not own.
