---
description: "Finalise the Verification and Validation plan for Phase 6 execution. The V&V plan has been seeded and updated throughout previous phases — this skill audits it for completeness, fills remaining gaps, adds validation scenarios from the ConOps, defines the test execution sequence, and prepares the plan for formal execution. Produces vv-results.md as an empty results ledger."
---

# SE V&V Plan

## Purpose

Finalise the Verification and Validation plan before test
execution begins. This skill does not create the V&V plan
from scratch — it was initiated in Phase 1 by `/se-nfr`,
extended in Phase 2 by `/se-req-quality`, and updated in
Phases 3 and 4. This skill audits the accumulated plan,
fills any remaining gaps, adds formal validation scenarios,
defines the execution sequence, and produces the results
ledger that will be populated during execution.

The distinction between verification and validation is
fundamental and must be maintained throughout:

**Verification** — Did we build the system right?
Confirms the system complies with its specified requirements.
Asks: *does the system meet the spec?*

**Validation** — Did we build the right system?
Confirms the system satisfies stakeholder needs in the
operational environment. Asks: *does the system solve
the actual problem?*

Both are required. A system that passes all verification
events but fails validation has been built correctly to
the wrong specification.

Output artifacts:

- `specs/system/09-vv/vv-plan.md` (finalised)
- `specs/system/09-vv/vv-results.md` (empty ledger)
- `specs/features/$FEATURE_ID/09-vv/vv-delta.md` (feature delta)

Updates:

- `specs/system/rtm.md`
- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `specs/system/09-vv/vv-plan.md` must exist.
  It will have been created and updated by earlier skills.
  Read it in full before doing anything else.
- `specs/system/04-requirements/requirements-register.md`
  must exist. Read it.
- `specs/system/02-nfr/quality-attributes.md`
  must exist. Read it.
- `specs/system/06-interfaces/icd.md` must exist.
  Read it — interfaces require verification events.
- `specs/system/01-conops/conops.md` must exist.
  Read it — operational scenarios drive validation events.
- `specs/system/00-stakeholders/needs-register.md`
  must exist. Read it — validation traces to needs,
  not requirements.
- `specs/system/rtm.md` must exist. Read it.

If `specs/system/09-vv/vv-plan.md` does not exist, stop. Tell
the user that the V&V plan should have been initiated by
`/thing.se-nfr` and extended by
`/thing.se-requirements`. Those skills
must be run before this one.

## Verification Methods

Apply these definitions precisely and consistently:

**Test (T)**
Execute the system or a component under defined conditions
and observe results against a measurable pass criterion.
Use when the requirement has a quantitative criterion that
can be confirmed by running the system.

**Analysis (A)**
Mathematical, logical, or model-based evaluation without
executing the system. Use for requirements that can be
confirmed by calculation, simulation, or reasoning from
known properties. Examples: timing analysis, reliability
modelling, static code analysis, mass budget calculation.

**Inspection (I)**
Visual or documented review of artifacts, code,
configuration, or physical items. Use for requirements
that can be confirmed by examining what was built without
running it. Examples: code review against coding standard,
review of wiring against schematic, review of label text.

**Demonstration (D)**
Operate the system to show a capability is present without
formal quantitative measurement. Use when the requirement
is qualitative and a witnessed operation is sufficient
evidence. Examples: demonstrating a user workflow,
showing a mode transition occurs, showing an alarm fires.

## Verification Stages

**Unit Test (UT)**
Individual component or module in isolation.
Typically automated. Developer-owned.

**Integration Test (IT)**
Interaction between two or more components or subsystems.
Confirms interface behaviour. May be automated or manual.

**System Test (ST)**
Full system against the complete requirements set.
Executed in a test environment representative of deployment.

**Acceptance Test (AT)**
Stakeholder validation in the operational environment
or a high-fidelity replica. Confirms the system solves
the real problem for real users. Owned jointly by
engineering and stakeholders.

## Baseline-Update Mode

The system-level artifacts this skill writes are a baseline: one
current description of the whole system, not a log of features.
This applies to:

- `specs/system/09-vv/vv-plan.md`
- `specs/system/09-vv/vv-results.md`

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
2. Verification entries, validation scenarios and results each
   live in one table for the whole system. Never recreate
   `vv-results.md` when it exists; add this feature's rows to its
   tables and keep earlier results. Never delete rows from either
   file.
3. Add new rows to the existing table. Edit changed rows in place;
   where the history matters, add
   "revised {YYYY-MM-DD}, {feature ID}" to the row's notes.
4. Retire rows that are no longer true: strike through the text
   (`~~…~~`), set the status to `Superseded` and say by what in
   Notes, and keep the row so its ID stays reserved. Record the
   reason in the feature delta, not in the baseline.
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
- `specs/system/05-architecture/`
- `specs/system/06-interfaces/`
- `specs/system/09-vv/`
- `specs/system/rtm.md` (threaded; this skill updates it)
- `specs/system/10-risk/risk-register.md` (threaded; this skill updates it)

Correct every statement in them that the feature has made
untrue, including in artifacts that earlier skills own.
Re-assess every risk, V&V entry or RTM row in the artifacts
above whose mitigation or subject the feature removes or
changes.

Finally, write the feature delta
`specs/features/$FEATURE_ID/09-vv/vv-delta.md` using the template
at `artifacts/baseline-delta.md`. If it already exists (this skill
ran earlier for the same feature), append to its tables. Record:

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

### Step 1 — Audit the Existing V&V Plan

Read the current `vv-plan.md` in full. For every entry,
check:

- [ ] Req ID is present and matches an entry in the
      requirements register or ICD
- [ ] Verification method is assigned (T/A/I/D) —
      not TBD
- [ ] Verification stage is assigned (UT/IT/ST/AT) —
      not TBD
- [ ] Pass criterion is defined and measurable —
      not TBD and not vague
- [ ] Responsible party is assigned — not TBD
      (at minimum: developer, test engineer, or
      external certification body)

For each entry with a TBD or missing field, flag it
as a gap requiring resolution in Step 2.

Also check coverage:

- Every requirement in the requirements register has
  at least one V&V plan entry
- Every Must Have NFR has at least one V&V plan entry
- Every interface in the ICD has at least one
  verification event
- No V&V plan entry references a requirement that
  does not exist in the requirements register

### Step 2 — Resolve Gaps

For each gap identified in Step 1, resolve it by asking
the user targeted questions. Do not ask generic questions —
ask specifically about the gap found.

For a missing verification method:
*"Requirement {REQ-ID} states '{requirement text}'.
How do you intend to confirm this is satisfied —
by running a test, by analysis, by inspection,
or by demonstration?"*

For a vague pass criterion:
*"The pass criterion for {REQ-ID} currently says
'{vague text}'. What is the specific, measurable
condition that would confirm this requirement is met?
For example: response time ≤ 200ms measured at the
API boundary under a load of 100 concurrent requests."*

For a missing responsible party:
*"Who is responsible for executing the verification
of {REQ-ID} — the development team, a dedicated
test engineer, or an external body such as a
certification authority?"*

For a missing V&V entry (requirement with no coverage):
*"Requirement {REQ-ID} has no verification event.
How will compliance be confirmed?"*

### Step 3 — Add Validation Scenarios

Verification confirms compliance with requirements.
Validation confirms the system solves the stakeholder's
actual problem. Validation is driven by the operational
scenarios in the ConOps, not by the requirements register.

Tell the user: *"We now need to define validation events —
tests conducted with real or representative users in
the operational environment that confirm the system
actually solves the problem it was built to solve.
These map to the operational scenarios we defined
in the ConOps."*

For each operational scenario in
`specs/system/01-conops/operational-scenarios.md`:

1. Can this scenario be executed as an acceptance test?
   If so, with whom (which stakeholder), in what environment,
   and what constitutes a successful outcome?
2. Are there scenarios that cannot be tested in a live
   environment and require a simulation or representative
   environment?
3. Are there scenarios that must be witnessed by a
   regulatory body or certification authority?

For each scenario that becomes a validation event:

- Assign a Validation ID: `VAL-{NNN}`
- Record the scenario ID it maps to
- Record the stakeholder who participates
- Record the environment required
- Record the success criterion in operational terms
  (not system terms — what the stakeholder observes,
  not what the system measures)

### Step 4 — Define Execution Sequence

Tell the user: *"Now we need to define the order in which
verification and validation events are executed. The sequence
matters — you cannot run system tests before unit tests pass,
and you cannot run acceptance tests in an environment that
hasn't been validated by system tests."*

Ask:

1. Are there any verification events that have hard
   prerequisites? (e.g. component X must pass IT before
   component Y's IT can begin)
2. What is the entry criterion for System Test?
   (e.g. all UT and IT events passed with no open
   critical defects)
3. What is the entry criterion for Acceptance Test?
   (e.g. all ST events passed, system deployed to
   production-equivalent environment)
4. Are there any verification events that can run
   in parallel?
5. Are there any events that must be witnessed or
   executed by an external party, and what lead time
   do they require?

Define the execution sequence as a set of stages with
entry criteria for each stage.

### Step 5 — Regulatory and Certification Events

If the constitution identifies applicable regulatory
frameworks, ask:

1. Which verification or validation events must be
   formally documented for regulatory submission?
2. Are there specific test protocols mandated by the
   applicable standard?
   (e.g. IEC 62304 requires software system testing
   to be conducted per a documented test plan;
   ISO 14971 requires risk control measures to be
   verified)
3. Must any test records be retained for a specific
   period?
4. Is there an independent verification and validation
   (IV&V) requirement — where a party independent of
   the development team must conduct or witness tests?

### Step 6 — Finalise the V&V Plan

Rewrite `specs/system/09-vv/vv-plan.md` as one plan for the
whole system (see Baseline-Update Mode) with the
complete, audited, gap-free content:

```markdown
# Verification and Validation Plan

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** FINALISED — ready for execution
**Last Updated By:** se-vvplan skill
**Version:** {increment from previous}

---

## Purpose

This plan defines how every requirement and stakeholder need
will be verified or validated. Verification confirms the system
meets its specification. Validation confirms the system solves
the stakeholder's actual problem.

This is a living document. Results are recorded in
`vv-results.md` as events are executed.

---

## Verification Methods

| Code | Method | When to Use |
| ---- | ------ | ----------- |
| T | Test | Quantitative criterion, system execution required |
| A | Analysis | Mathematical or model-based, no execution required |
| I | Inspection | Review of artifact, no execution required |
| D | Demonstration | Qualitative, witnessed operation sufficient |

---

## Verification Stages

| Code | Stage | Entry Criterion |
| ---- | ----- | --------------- |
| UT | Unit Test | Component implementation complete |
| IT | Integration Test | All contributing UT events passed |
| ST | System Test | All IT events passed, system integrated |
| AT | Acceptance Test | {entry criterion from Step 4} |

---

## Execution Sequence

### Stage 1 — Unit Test
**Entry Criterion:** {criterion}
**Exit Criterion:** {criterion — e.g. all UT events pass,
zero open critical defects}

### Stage 2 — Integration Test
**Entry Criterion:** {criterion}
**Exit Criterion:** {criterion}

### Stage 3 — System Test
**Entry Criterion:** {criterion}
**Exit Criterion:** {criterion}

### Stage 4 — Acceptance Test
**Entry Criterion:** {criterion}
**Exit Criterion:** {criterion}

---

## Verification Plan

Valid Status values: `Planned` | `Pass` | `Fail` | `Waived` | `Carried Forward`

When Status = `Carried Forward`, the `Notes` field must contain:
(a) the rationale for deferral and (b) the target Feature ID expected to close this entry.

| V-ID | Req ID | Type | Description | Method | Stage | Pass Criterion | Responsible | Feature | Status | Notes |
| ---- | ------ | ---- | ----------- | ------ | ----- | -------------- | ----------- | ------- | ------ | ----- |
| VVP-001 | SRS-001 | Functional | {desc} | T | ST | {criterion} | {owner} | {feature-id} | Planned | |
| VVP-002 | NFR-PERF-001 | NFR | {desc} | T | ST | {criterion} | {owner} | {feature-id} | Planned | |
| VVP-003 | IF-001 | Interface | {desc} | T | IT | {criterion} | {owner} | {feature-id} | Planned | |

---

## Validation Plan

| VAL-ID | Scenario ID | Description | Stakeholder | Environment | Success Criterion | Responsible | Feature | Status | Notes |
| ------ | ----------- | ----------- | ----------- | ----------- | ----------------- | ----------- | ------- | ------ | ----- |
| VAL-001 | SCN-001 | {desc} | {STK-ID} | {environment} | {operational success criterion} | {owner} | {feature-id} | Planned | |

---

## Regulatory and Certification Events

{If applicable — list events requiring regulatory
documentation, independent execution, or retention.
If not applicable: state explicitly.}

| Event ID | Standard Clause | Description | Documentation Required | Retention Period |
| -------- | --------------- | ----------- | ---------------------- | ---------------- |

---

## Coverage Summary

| Category | Total Items | V&V Entries | Coverage |
| -------- | ----------- | ----------- | -------- |
| Functional Requirements | {N} | {N} | {%} |
| Must Have NFRs | {N} | {N} | {%} |
| Interfaces | {N} | {N} | {%} |
| Operational Scenarios (Validation) | {N} | {N} | {%} |
| **Total** | {N} | {N} | **{%}** |

---

## Open Items

{Any items that could not be resolved during this skill
and require follow-up before execution begins}

| ID | Item | Owner | Must Resolve Before |
| -- | ---- | ----- | ------------------- |
```

### Step 7 — Create V&V Results Ledger

If `specs/system/09-vv/vv-results.md` exists, add this
feature's rows to its tables and keep all earlier results.
Otherwise create it as an
empty ledger ready to receive results during execution:

```markdown
# Verification and Validation Results

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** OPEN — execution in progress
**Last Updated By:** {updated as results are recorded}

---

## Summary

| Category | Total | Passed | Failed | Waived | Not Run |
| -------- | ----- | ------ | ------ | ------ | ------- |
| Verification | {N} | 0 | 0 | 0 | {N} |
| Validation | {N} | 0 | 0 | 0 | {N} |

---

## Verification Results

| V-ID | Req ID | Description | Method | Stage | Pass Criterion | Feature | Result | Date | Executed By | Defect Ref | Notes |
| ---- | ------ | ----------- | ------ | ----- | -------------- | ------- | ------ | ---- | ----------- | ---------- | ----- |
| VVP-001 | SRS-001 | {desc} | T | ST | {criterion} | {feature-id} | Not Run | — | — | — | |

---

## Validation Results

| VAL-ID | Scenario ID | Description | Stakeholder | Feature | Result | Date | Witnessed By | Notes |
| ------ | ----------- | ----------- | ----------- | ------- | ------ | ---- | ------------ | ----- |
| VAL-001 | SCN-001 | {desc} | {STK-ID} | {feature-id} | Not Run | — | — | |

---

## Defect Register

{Defects raised during V&V execution. Each failed
verification event must reference a defect here.}

| Defect ID | V-ID | Description | Severity | Status | Resolution |
| --------- | ---- | ----------- | -------- | ------ | ---------- |

---

## Waivers

{Verification events that were waived — not executed
but accepted. Each waiver requires documented rationale
and approval.}

| Waiver ID | V-ID | Rationale | Approved By | Risk Accepted |
| --------- | ---- | --------- | ----------- | ------------- |
```

### Step 8 — Update Threaded Artifacts

#### RTM Update

For each requirement in `specs/system/rtm.md`,
update the V&V Reference column with the VVP-ID of
its verification event. Where a requirement has multiple
verification events, list all VVP-IDs.

#### Risk Register Update

Add entries for:

- Any requirement whose verification method is Test but
  where the test environment does not yet exist
- Any validation scenario requiring stakeholder
  participation where availability is uncertain
- Any regulatory certification event with a long
  lead time or external dependency
- Any requirement that was waived rather than verified,
  recording the accepted risk

### Step 9 — Confirm Completion

Report to the user:

1. Total verification events in the finalised plan
2. Total validation events
3. Coverage percentages by category
4. Any remaining open items that must be resolved
   before execution begins
5. Number of regulatory/certification events identified
6. RTM V&V reference coverage percentage
7. Number of risk entries added
8. Recommended next skill:
   `/thing.se-rtm`

Also report the feature delta path, the number of baseline
items added, changed and retired, and every statement corrected
in an artifact this skill does not own.
