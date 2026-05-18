---
description: "Run a phase gate review for the specified phase number. Checks that all required artifacts exist, are complete, and that threaded artifacts are current before allowing progression to the next phase. Produces a gate report. Usage: /thing.se-gate {phase-number}"
---

# SE Gate

## Purpose

Enforce phase completion before progression. A phase gate is not
a progress review — it is a binary pass/fail check against defined
completion criteria. A phase that does not pass its gate does not
close. Work does not proceed to the next phase until the gate passes.

Gates check two things:

1. **Artifact existence and completeness** — required sequenced
   artifacts for the completed phase exist and contain no
   unresolved TBDs in mandatory fields
2. **Threaded artifact currency** — threaded artifacts have been
   updated to reflect all work done in the completed phase

Output artifacts:

- `specs/features/$FEATURE_ID/gate-reports/gate-{phase-number}-{status}.md`

## Pre-Conditions

- `$ARGUMENTS` must contain the phase number to gate.
  Valid values: 0, 1, 2, 3, 4, 5, 6
- `.specify/memory/constitution.md` must exist. Read it.
- The feature ID must be determinable from context or
  from `$ARGUMENTS`.

If phase number is missing, ask for it before proceeding.

## Gate Criteria by Phase

---

### Phase 0 Gate — Problem Space

**Checks:**

#### Constitution

- [ ] `.specify/memory/constitution.md` exists
- [ ] System name is populated (not TBD)
- [ ] Problem statement is populated (not TBD)
- [ ] Safety classification is stated
- [ ] SE workflow obligations section is present

#### Stakeholders

- [ ] `specs/system/00-stakeholders/stakeholder-register.md`
      exists and contains at least one stakeholder
- [ ] At least one persona file exists in
      `specs/system/00-stakeholders/personas/`
- [ ] At least one empathy map exists in
      `specs/system/00-stakeholders/empathy-maps/`
- [ ] `specs/system/00-stakeholders/needs-register.md`
      exists and contains at least one need per stakeholder
- [ ] No stakeholder in the register has zero needs captured

#### ConOps

- [ ] `specs/system/01-conops/conops.md` exists
- [ ] At least one mode of operation is defined
- [ ] At least one success criterion is defined
- [ ] `specs/system/01-conops/operational-scenarios.md`
      exists and contains at least one normal success scenario

#### Threaded Artifacts

- [ ] `specs/system/10-risk/risk-register.md` exists
- [ ] Risk register contains at least one entry
- [ ] All risk entries have a probability, consequence,
      and handling strategy (not TBD)

#### Conflicts

- [ ] All needs conflicts identified in the needs register
      have been carried into the ConOps or marked as
      resolved with rationale

---

### Phase 1 Gate — Solution Space

**Checks:**

#### NFR

- [ ] `specs/system/02-nfr/quality-attributes.md` exists
- [ ] At least one NFR exists in each applicable category
      (categories marked not applicable must have explicit
      confirmation in the document)
- [ ] Every Must Have NFR has a verification method
      (not TBD)
- [ ] No NFR conflict is unresolved

#### Explore

- [ ] `specs/features/$FEATURE_ID/03-explore/explore-summary.md` exists
- [ ] At least two options are defined (or a single-option
      justification is documented with explicit rationale
      for why alternatives were not viable)
- [ ] At least one integration point is scheduled with a
      defined question and evidence threshold
- [ ] A convergence target date or milestone is recorded
- [ ] Each active option has:
      - [ ] `description.md`
      - [ ] `assumptions.md` with at least one entry
      - [ ] `tasks.md` with at least one task targeting IP-001
      - [ ] `evaluation-log.md` (may be empty at this stage)
- [ ] `elimination-record.md` exists (may be empty at this stage)

#### Threaded Artifacts

- [ ] `specs/system/09-vv/vv-plan.md` exists
- [ ] V&V plan contains one entry per Must Have NFR
- [ ] `specs/system/10-risk/risk-register.md` has been
      updated since Phase 0 gate (check Last Updated date)
- [ ] Risk entries added for high-risk option assumptions

---

### Phase 2 Gate — Requirements

**Checks:**

#### Functional Requirements

- [ ] `specs/system/04-requirements/requirements-register.md`
      exists with one row per requirement
- [ ] No requirement contains the phrase "TBD" or "to be
      determined" in mandatory fields
- [ ] Every requirement has a unique ID

#### Requirements Quality

- [ ] `specs/system/04-requirements/req-quality-report.md`
      exists
- [ ] The count of requirements assessed in the quality
      report matches the count of requirements in the
      requirements register (report is not stale). If counts
      differ, re-run `/thing.se-req-quality` before
      this gate can pass.
- [ ] No requirement is flagged as failing Necessary,
      Unambiguous, or Verifiable attributes
- [ ] All Should Fix items in the quality report with
      Status = `Open` have a documented Acceptance Rationale.
      Should Fix items that have been Open across multiple
      features without rationale are a gate failure — they
      must be either resolved or formally accepted.

#### Threaded Artifacts

- [ ] `specs/system/rtm.md` exists
- [ ] RTM contains one row per requirement in the
      requirements register
- [ ] Every RTM row has a requirement ID and description
      (design allocation and test reference may be TBD
      at this stage)
- [ ] `specs/system/09-vv/vv-plan.md` has been updated:
      one entry per functional requirement
- [ ] Every V&V plan entry for a Must Have requirement
      has a verification method (not TBD)
- [ ] `specs/system/10-risk/risk-register.md` has been
      reviewed against the completed requirements register

---

### Phase 3 Gate — Architecture

**Checks:**

#### Architecture

- [ ] `specs/system/05-architecture/architecture.md` exists
- [ ] At minimum these views exist as Mermaid diagrams:
      - [ ] `specs/system/05-architecture/views/context.md` (C4 Level 1)
      - [ ] `specs/system/05-architecture/views/container.md` (C4 Level 2)
      - [ ] `specs/system/05-architecture/views/functional-flow.md`
- [ ] At least one ADR exists in
      `specs/system/05-architecture/decisions/`
- [ ] Every significant design decision referenced in
      `architecture.md` has a corresponding ADR

#### Interfaces

- [ ] `specs/system/06-interfaces/icd.md` exists
- [ ] Every external system named in the ConOps has
      an interface entry in the ICD
- [ ] Every interface entry has a defined type, provider,
      consumer, and protocol/format (not all TBD)

#### Explore Resolution

- [ ] If multiple options were active at Phase 1 gate,
      at least one integration point has been reviewed
- [ ] Any eliminated options have entries in
      `specs/features/$FEATURE_ID/03-explore/elimination-record.md`
      with evidence cited

#### Threaded Artifacts

- [ ] `specs/system/rtm.md` has been updated:
      design element column populated for all requirements
- [ ] `specs/system/09-vv/vv-plan.md` has been updated:
      verification stage populated for all Must Have items
- [ ] `specs/system/10-risk/risk-register.md` updated
      with architecture-derived risks

---

### Phase 4 Gate — Implementation Planning

**Checks:**

#### Tasks

- [ ] `specs/features/$FEATURE_ID/08-tasks/tasks.md` exists
- [ ] Every requirement in the requirements register
      is addressed by at least one task
- [ ] No task references a requirement that does not
      exist in the requirements register
- [ ] `/thing.se-analyze` (for SE features) or
      `/speckit.analyze` (for standard Spec-Kit features)
      has been run and its output reviewed — no critical
      coverage gaps outstanding

#### Explore Resolution

- [ ] All integration points scheduled before convergence
      target have been reviewed
- [ ] Solution set has converged to one option, or a
      documented exception exists explaining why convergence
      has been deferred

#### Threaded Artifacts

- [ ] `specs/system/rtm.md` updated: task reference
      column populated for all requirements
- [ ] `specs/system/10-risk/risk-register.md` reviewed

---

### Phase 5 Gate — Implementation

**Checks:**

#### Implementation

- [ ] All tasks in `specs/features/$FEATURE_ID/08-tasks/tasks.md`
      are marked complete or have a documented exception
- [ ] No requirement in the requirements register is
      unimplemented without a documented exception

#### Threaded Artifacts

- [ ] `specs/system/rtm.md` updated: implementation
      reference column populated
- [ ] `specs/system/10-risk/risk-register.md` reviewed

---

### Phase 6 Gate — Final

**Checks:**

#### V&V

- [ ] `specs/system/09-vv/vv-plan.md` exists and is finalised
- [ ] `specs/system/09-vv/vv-results.md` exists

**Feature-scoped V&V entries** — entries whose `Feature`
column matches the current feature ID:

- [ ] Every feature-scoped entry has a result:
      `Pass`, `Fail`, or `Waived`. `Planned` is a gate failure.
- [ ] No feature-scoped Must Have requirement has a `Fail`
      result without a documented disposition (accepted risk
      or open defect with owner and target resolution date)

**Pre-existing V&V entries** — entries whose `Feature`
column does not match the current feature ID:

- [ ] Every pre-existing entry is in one of these states:
      `Pass`, `Fail`, `Waived`, or `Carried Forward`.
      `Planned` is a gate failure unless the entry is out of
      scope for this feature and has been explicitly marked
      `Carried Forward`.
- [ ] Every `Carried Forward` entry has a non-empty `Notes`
      field with: (a) the rationale for deferral and (b) the
      target Feature ID expected to close it.
- [ ] No pre-existing entry has regressed (moved from `Pass`
      or `Waived` to `Fail` or `Planned`) without explanation.

**Acceptance tests:**

- [ ] All acceptance test scenarios from the ConOps that are
      in scope for the current feature have been executed

#### RTM

- [ ] `specs/system/rtm.md` is complete:
      all columns populated for all requirements
- [ ] RTM coverage is 100% — every requirement has at
      least one passing verification event
- [ ] No requirement is untraced to a stakeholder need

#### Risk

- [ ] `specs/system/10-risk/risk-register.md`:
      every risk has a `Status` value of `Closed`,
      `Accepted`, or `Carried Forward`. `Open` or `Mitigated`
      (without a final disposition) is a gate failure.
      Note: `Strategy` (Avoid/Mitigate/Transfer/Accept/Watch)
      is the handling approach — `Status` is the disposition;
      they are different columns, check `Status`.
- [ ] Every `Carried Forward` risk has a non-empty `Notes`
      field with: (a) the rationale for deferral and (b) the
      owner and review cadence
- [ ] No unmitigated High risk (level ≥ 15) remains open
      without explicit acceptance and sign-off recorded in
      the Escalation Register

#### Explore

- [ ] Solution set has converged to exactly one option
- [ ] `specs/features/$FEATURE_ID/03-explore/elimination-record.md`
      accounts for all options that were defined but not selected

---

## Instructions

### Step 1 — Identify Phase and Feature

Read `$ARGUMENTS` to determine:

- The phase number to gate (0–6)
- The feature ID (if not in arguments, infer from context
  or ask)

### Step 2 — Run Gate Checks

Work through every check in the criteria for the specified
phase. For each check:

- Examine the relevant artifact
- Record PASS, FAIL, or NOT APPLICABLE with a brief note
- For FAIL: state specifically what is missing or incomplete
  and what action is needed to resolve it

Do not skip checks. Do not mark a check PASS without
examining the artifact.

### Step 3 — Calculate Gate Result

The gate PASSES only if:

- All mandatory checks are PASS or NOT APPLICABLE
- No check is FAIL

The gate FAILS if any check is FAIL.

A gate with outstanding items marked NOT APPLICABLE must
include a documented rationale for each NA — silence is
not justification.

### Step 4 — Write Gate Report

Write `specs/features/$FEATURE_ID/gate-reports/gate-{N}-{pass|fail}.md`:

```markdown
# Phase {N} Gate Report

**System:** {system name}
**Feature:** {feature ID and name}
**Phase:** {phase number and name}
**Date:** {date}
**Result:** PASS | FAIL
**Conducted By:** se-gate skill

---

## Summary

**Total Checks:** {N}
**Passed:** {N}
**Failed:** {N}
**Not Applicable:** {N}

{One paragraph summary of the gate result and the most
important findings.}

---

## Check Results

| Check | Artifact | Result | Notes |
| ----- | -------- | ------ | ----- |
| Constitution exists | `.specify/memory/constitution.md` | PASS | |
| ... | | | |

---

## Failures — Action Required

{One section per failed check}

### FAIL: {check description}

**Artifact:** {path}
**Finding:** {what is missing or incomplete}
**Action Required:** {what must be done}
**Owner:** TBD
**Must Resolve Before:** Re-running this gate

---

## Not Applicable Items

| Check | Rationale |
| ----- | --------- |
| | |

---

## Gate Decision

**Result:** PASS | FAIL

{If PASS:}
Phase {N} is complete. Proceed to Phase {N+1}.
Recommended next skill: {skill name}

{If FAIL:}
Phase {N} is NOT complete. The {N} failures listed above
must be resolved before this gate can pass.
Re-run `/thing.se-gate {N}` after resolving
all failures.
```

### Step 5 — Report to User

State the gate result clearly — PASS or FAIL — before
any other content.

If PASS: state the recommended next skill.
If FAIL: list the failures grouped by artifact, with
specific remediation actions. Do not bury the failures
in a long report — lead with them.
