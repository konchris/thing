---
description: "Derive functional requirements from SE artifacts (ConOps, NFRs, explore convergence decision) and apply IEEE 29148 quality checks. Produces the requirements register, seeds the V&V plan with one entry per requirement, and initialises the RTM. This skill replaces /speckit-specify for SE features, which have no spec.md."
---

# SE Requirements

## Purpose

Derive the complete set of functional requirements for an SE feature
from the operational and solution artifacts produced in Phases 0 and 1.
This skill is the requirements elicitation step for SE features — it
does the job that `/speckit-specify` does for standard Spec-Kit
features, but reads SE artifacts (ConOps, NFRs, explore summary)
rather than a `spec.md`.

Requirements are derived from three sources in priority order:

1. **Operational scenarios** — each scenario step that the system
   must perform becomes a candidate functional requirement
2. **NFR-driven requirements** — each quality attribute may impose
   functional obligations (e.g., an availability NFR drives a health
   monitoring requirement)
3. **Explore convergence** — the selected solution option may
   introduce new capabilities not yet captured in the scenarios

All derived requirements are checked against the eight IEEE 29148
quality attributes inline. A requirement that fails Verifiable is
not placed in the register — it is rewritten until it passes.

Output artifacts:

- `specs/system/04-requirements/requirements-register.md`
- `specs/features/$FEATURE_ID/04-requirements/requirements-delta.md` (feature delta)

Initiates or updates:

- `specs/system/09-vv/vv-plan.md` (one entry per requirement)
- `specs/system/rtm.md` (one row per requirement)
- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it.
- `specs/system/00-stakeholders/needs-register.md` must exist.
  Read it. Every requirement must trace to a need.
- `specs/system/01-conops/conops.md` must exist. Read it.
- `specs/system/01-conops/operational-scenarios.md` must exist.
  Read it. This is the primary source of functional requirements.
- `specs/system/02-nfr/quality-attributes.md` must exist.
  Read it. NFRs must not be duplicated as functional requirements,
  but they drive derived functional requirements.
- `specs/features/$FEATURE_ID/03-explore/explore-summary.md` — read
  if it exists. The convergence decision identifies the selected
  solution option and any capability requirements it introduces.

If any of the first five pre-conditions is not met, stop and tell
the user which skill must be run first.

## The Eight IEEE 29148 Quality Attributes

Apply these inline during derivation. Do not place a requirement
in the register until it passes all eight.

**1. Necessary** — Addresses a stakeholder need or is derived from
another requirement. Not gold-plating. Every requirement must trace
to a NEED-ID.

**2. Unambiguous** — Has exactly one interpretation. No subjective
qualifiers ("fast", "easy", "appropriate"), no undefined pronouns,
no vague quantifiers ("several", "many").

**3. Verifiable** — A pass/fail criterion can be defined. A feasible
verification method (Test, Analysis, Inspection, or Demonstration)
exists. *A requirement that cannot be tested is not a requirement.*

**4. Consistent** — Does not contradict any other requirement, any
NFR, or any statement in the constitution.

**5. Complete** — Stands alone. No TBDs, no undefined references,
all conditions and error cases stated.

**6. Singular** — States exactly one thing. No "and" connecting
distinct capabilities. No multiple "shall" clauses.

**7. Feasible** — Can be implemented within the constraints in the
constitution: technical approach, budget, schedule, regulatory.

**8. Traceable** — Has a unique ID and a documented source (NEED-ID
or parent requirement ID).

## Baseline-Update Mode

The system-level artifacts this skill writes are a baseline: one
current description of the whole system, not a log of features.
This applies to:

- `specs/system/04-requirements/requirements-register.md`

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
2. Requirements and coverage gaps each live in one table for the
   whole system. The statistics describe the whole register, not
   this feature's additions.
3. Add new rows to the existing table. Edit changed rows in place;
   where the history matters, add
   "revised {YYYY-MM-DD}, {feature ID}" to the row's notes.
4. Retire rows that are no longer true: strike through the text
   (`~~…~~`), set the Quality Status to `Retired`, and keep the
   row so its ID stays reserved. Record the reason in the feature
   delta, not in the baseline.
5. Rewrite prose so it describes the system as it is after this
   feature. A sentence this feature has made untrue is corrected,
   not left standing beside a newer one.
6. Keep IDs stable. Never renumber and never reuse an ID. New IDs
   start at the highest existing ID + 1.
7. Set the header to `**Baseline as of:** {feature ID}, {date}`.

Then list the services, interfaces, behaviours and constraints
this feature adds, removes or changes. Search the artifacts of
this and earlier development steps for statements about each
one:

- `specs/system/00-stakeholders/`
- `specs/system/01-conops/`
- `specs/system/02-nfr/`
- `specs/system/04-requirements/`
- `specs/system/09-vv/vv-plan.md` (threaded; this skill updates it)
- `specs/system/rtm.md` (threaded; this skill updates it)
- `specs/system/10-risk/risk-register.md` (threaded; this skill updates it)

Correct every statement in them that the feature has made
untrue, including in artifacts that earlier skills own.
Re-assess every risk, V&V entry or RTM row in the artifacts
above whose mitigation or subject the feature removes or
changes.

Do not search or edit the artifacts of later development steps
(`05-architecture/`, `06-interfaces/` under `specs/system/`).
Their skills bring them up to date when they run for this feature,
and each phase gate checks them.

Finally, write the feature delta
`specs/features/$FEATURE_ID/04-requirements/requirements-delta.md`
using the template at `artifacts/baseline-delta.md`. If it already
exists (this skill ran earlier for the same feature), append to
its tables. Record:

- **System Changes** — the list above
- **Baseline Edits** — one row per item or prose section added,
  changed or retired, in any artifact listed above, with the
  reason
- **Statements Checked** — existing statements reviewed against
  the System Changes and left unchanged because they are still
  true

Write the delta on the first feature too; there every edit is
`Added`.

## Instructions

### Step 1 — Read and Internalise Context

Read all pre-condition artifacts. Build an internal model of:

- The system's mission and boundary (constitution)
- All stakeholder needs and their IDs (needs register)
- The operational modes and scenarios (ConOps)
- The system quality obligations (NFRs)
- The selected solution approach (explore summary, if present)

Greet the user and state: *"I'll now derive the functional requirements
for this feature from the ConOps scenarios, NFRs, and explore
convergence decision, and validate each against IEEE 29148. I'll
work through each source in turn."*

### Step 2 — Derive Requirements from Operational Scenarios

Work through `operational-scenarios.md` scenario by scenario.

For each scenario:

1. Read the scenario steps
2. For each step where the **system** must take an action or
   maintain a property, draft a candidate requirement
3. Apply the eight quality checks to each candidate
4. If any check fails, rewrite the requirement until it passes,
   or determine that the scenario step does not yield a standalone
   requirement (record rationale if so)
5. Assign a Req ID: `FR-{NNN}` starting at 001
6. Record the source scenario: `SCN-NNN`
7. Record the source need: trace through the scenario to the
   need it satisfies using the needs register

Also check: does each scenario's **success criterion** imply a
measurable system property? If so, derive a requirement from it.

Typical scenario-derived requirement forms:

- `The system shall [action] within [time] when [trigger].`
- `The system shall [action] when [condition] and [condition].`
- `The system shall detect [event] and [response] within [time].`

### Step 3 — Derive NFR-Driven Functional Requirements

Work through `quality-attributes.md`.

For each NFR that is a Must Have or Should Have:

1. Ask: does this NFR impose a functional obligation on the system
   beyond "be fast" or "be reliable"? Examples:
   - Availability NFR → may drive a health monitoring requirement
   - Security NFR → may drive an authentication requirement
   - Maintainability NFR → may drive a diagnostic interface requirement
   - Safety NFR → may drive a watchdog or fail-safe mode requirement
2. If yes, draft the functional requirement
3. Apply the eight quality checks
4. Assign a Req ID: `FR-{NNN}` (continuing from Step 2)
5. Record the source NFR ID

**Do not duplicate NFRs as functional requirements.** An availability
NFR of 99.9% is itself a requirement. The *driven* functional
requirement is the mechanism that enables it (e.g., a health check
endpoint).

### Step 4 — Derive Requirements from Explore Convergence

If `specs/features/$FEATURE_ID/03-explore/explore-summary.md`
exists and records a convergence decision:

1. Read the selected option's description and assumptions
2. Identify any capabilities the selected option requires that
   are not already captured in the operational scenarios
3. Draft requirements for those capabilities
4. Apply the eight quality checks
5. Assign Req IDs and trace to the explore summary as source
   (format: `EXPLORE: {option ID}`)

### Step 5 — Check Coverage

For each need in the needs register:

- Is at least one functional requirement traceable to this need?
- If not, is the need addressed by an NFR?
- If the need has no coverage at all, flag it as a gap and
  draft a requirement to close it

Report coverage gaps to the user: *"The following needs have no
requirement coverage. I'll draft requirements for each — please
review them."*

### Step 6 — Check Cross-Requirement Consistency

After all requirements are derived:

- Do any two requirements contradict each other?
- Does any requirement contradict an NFR or constitution constraint?
- Are there any duplicate requirements covering the same need?

Resolve duplicates by merging or eliminating the weaker statement.
Flag unresolvable contradictions for the user to decide.

### Step 7 — Write the Requirements Register

Write `specs/system/04-requirements/requirements-register.md`, or
merge into it if it exists (see Baseline-Update Mode):

```markdown
# Requirements Register

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** DRAFT
**Last Updated By:** se-requirements skill

---

*This register is the authoritative list of all requirements for
the system. It is the source of truth for the RTM, V&V plan,
and phase gate checks. Every requirement must appear here before
it can be allocated to a design element or assigned a verification
event.*

---

## Register

| Req ID | Type | Requirement Text | Source | Traced Need | Priority | Quality Status | V&V Entry | RTM Row |
| ------ | ---- | ---------------- | ------ | ----------- | -------- | -------------- | --------- | ------- |
| FR-001 | Functional | {full requirement text} | SCN-001 | NEED-STK-001-001 | Must Have | PASS | ✓ | ✓ |

---

## Statistics

- Total requirements: {N}
- Derived from scenarios: {N}
- Derived from NFRs: {N}
- Derived from explore convergence: {N}
- Quality PASS: {N}
- Needs fully covered: {N} of {total} ({%})
- Coverage gaps: {N} — must resolve before Phase 2 gate

---

## Coverage Gaps

| Need ID | Need Description | Gap | Action |
| ------- | ---------------- | --- | ------ |
```

### Step 8 — Seed or Update the V&V Plan

Open `specs/system/09-vv/vv-plan.md`. It was created by
`/se-nfr` with entries for all NFRs.

For each functional requirement in the register, add a row:

- Req ID
- Type: Functional
- Short description
- Verification Method: infer from the verifiable check —
  measurable criterion → Test; structural property → Inspection;
  mathematical → Analysis; observable behaviour → Demonstration
- Stage: System Test (default) unless the requirement clearly
  belongs at unit or integration level
- Pass Criterion: the measurable criterion from the requirement
- Feature: the current feature ID (e.g. `007-python312-upgrade`)
- Status: Planned
- Notes: (blank)

### Step 9 — Initialise or Update the RTM

Open or create `specs/system/rtm.md`.

For each requirement, add a row:

```markdown
# Requirements Traceability Matrix

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** ACTIVE — updated continuously
**Last Updated By:** se-requirements skill

---

## Coverage

- Total requirements: {N}
- Traced to need: {N} ({%})
- Allocated to design: 0 (0%) — populated in Phase 3
- Covered by task: 0 (0%) — populated in Phase 4
- Verified: 0 (0%) — populated in Phase 6

---

## Matrix

| Req ID | Requirement Title | Source Need | Design Element | Task Ref | V&V Ref | Verification Status |
| ------ | ----------------- | ----------- | -------------- | -------- | ------- | ------------------- |
| FR-001 | {title} | NEED-STK-001-001 | TBD | TBD | VVP-{NNN} | Planned |
```

### Step 10 — Update Risk Register

**Risk ID assignment:** Before appending new entries, scan
the register for all existing `RISK-NNN` IDs and determine
the maximum N. Assign new IDs starting from `max_N + 1`.
Never derive the next ID from the last row alone — use the
maximum across the entire document to prevent collisions.
Set `Status` = `Open` and `Residual P`, `Residual C`,
`Residual Level` = `—` for all newly created entries.
Update the Summary table counts after adding entries.

Add risk entries to `specs/system/10-risk/risk-register.md` for:

- Any coverage gap — a need with no requirement
- Any requirement with a weak Feasible assessment (marked WARN)
- Any unresolved consistency conflict
- Any requirement where the verification method is TBD

### Step 11 — Confirm Completion

Report to the user:

1. Total requirements derived and registered
2. Breakdown by source: scenarios / NFRs / explore
3. Need coverage: N of M needs covered (list any gaps)
4. Count of V&V plan entries added
5. RTM row count and initial coverage percentage
6. Any risks added to the register
7. Recommended next skill: `/thing.se-req-quality`

Also report the feature delta path, the number of baseline
items added, changed and retired, and every statement corrected
in an artifact this skill does not own.

**Stale quality report warning:** If
`specs/system/04-requirements/req-quality-report.md`
already exists (this is a subsequent feature run, not the
first), state explicitly:

> "The requirements quality report exists from a prior run
> and does not yet cover the {N} new requirement(s) added
> in this session. The Phase 2 gate checks that the report
> covers every requirement in the register. You must run
> `/thing.se-req-quality` before Phase 2 can pass."
