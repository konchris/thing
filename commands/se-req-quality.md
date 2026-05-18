---
description: "Run an IEEE 29148 quality gate on the requirements register produced by /se-requirements. Checks each requirement against eight quality attributes: Necessary, Unambiguous, Verifiable, Consistent, Complete, Singular, Feasible, and Traceable. Produces a quality report and updates the requirements register with quality status."
---

# SE Requirements Quality

## Purpose

Apply the eight IEEE 29148 quality attributes to every requirement
in the requirements register. Requirements that do not meet these
attributes are not requirements — they are statements of intent that
will cause problems downstream: untestable verification events,
ambiguous implementation, and failed audits.

This skill is a quality gate, not a requirements creator. The
requirements register already exists (produced by `/se-requirements`).
This skill validates it, produces a quality report, and marks each
requirement with its quality status.

**A requirement that fails Verifiable is incomplete by definition.
It must be fixed before Phase 2 closes.**

Output artifacts:

- `specs/system/04-requirements/req-quality-report.md`

Updates:

- `specs/system/04-requirements/requirements-register.md`
  (quality status column updated for each requirement)
- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `specs/system/04-requirements/requirements-register.md`
  must exist. This is the output of `/thing.se-requirements`.
- `specs/system/00-stakeholders/needs-register.md`
  must exist. Required for traceability checking.
- `specs/system/09-vv/vv-plan.md` must exist.
  Created by `/thing.se-nfr` and extended by
  `/thing.se-requirements`.
- `specs/system/rtm.md` must exist.
  Created by `/thing.se-requirements`.

If `requirements-register.md` does not exist, stop and tell the
user to run `/thing.se-requirements` first.

## The Eight Quality Attributes

Apply these definitions precisely and consistently:

### 1. Necessary

The requirement addresses a stakeholder need or is derived
from another requirement. It is not gold-plating.
**Test:** Can this requirement be traced to a NEED-ID in the
needs register, or to another requirement that itself traces
to a need? If not, it may be unnecessary.

### 2. Unambiguous

The requirement has exactly one possible interpretation.
Every reader — engineer, tester, auditor — would understand
it identically.
**Failure patterns to detect:**

- Subjective qualifiers: "fast", "easy", "user-friendly",
  "appropriate", "adequate", "robust", "flexible"
- Undefined pronouns: "it", "they", "the system" (when
  multiple systems are in scope)
- Undefined acronyms or terms not in the constitution
- Vague quantifiers: "several", "many", "some", "few"
- Passive voice obscuring responsibility: "shall be displayed"
  (by whom? to whom?)

### 3. Verifiable

There exists a feasible method — Test, Analysis, Inspection,
or Demonstration — by which compliance can be objectively
confirmed. A pass/fail criterion can be defined.
**Failure patterns to detect:**

- No measurable criterion: "the system shall be fast"
- Criterion that cannot be measured: "the user shall be satisfied"
- Verification would require infinite testing: "the system
  shall never fail"
- Criterion depends on undefined reference: "shall meet
  industry best practice"

### 4. Consistent

The requirement does not contradict any other requirement
or any statement in the constitution, ConOps, or NFR document.
**Check:** Cross-reference against other requirements and
the quality attributes document for conflicts.

### 5. Complete

The requirement stands alone. It contains no TBDs, no
placeholders, no references to information that does not
yet exist. All conditions and exceptions are stated.
**Failure patterns to detect:**

- "TBD", "TBC", "to be determined", "to be confirmed"
- "as specified elsewhere" without a specific reference
- Conditions not covered: "under normal conditions" without
  defining abnormal
- Missing error handling: requirement describes success
  path only

### 6. Singular

The requirement states exactly one thing. It cannot be
decomposed into two independent requirements.
**Failure patterns to detect:**

- "and" connecting two distinct capabilities
- "or" implying optionality that should be a separate
  requirement
- Multiple "shall" clauses in one statement

### 7. Feasible

The requirement can be implemented within the known
constraints of the project: technical, budgetary, schedule,
and regulatory.
**Test:** Is there a known technical approach to satisfy
this requirement? Does it conflict with any constraint
in the constitution?

### 8. Traceable

The requirement has a unique identifier and a documented
source — either a stakeholder need ID or a parent requirement
ID from which it is derived.
**Note:** At this stage in the workflow, traceability means
forward traceability to a need. Backward traceability
(to design and tests) will be added in later phases via
the RTM.

---

## Instructions

### Step 1 — Read Context

Read all pre-condition artifacts. Note:

- All stakeholder needs and their IDs from the needs register
- All NFRs from `quality-attributes.md` — these are already
  requirements and should not be duplicated in `spec.md`,
  but functional requirements must not contradict them
- The constitution's engineering principles and constraints

### Step 2 — Load Requirements from the Register

Read `specs/system/04-requirements/requirements-register.md`
and load every requirement from the register table.

For each requirement:

- Note its Req ID (already assigned by `/se-requirements`)
- Note its full requirement text
- Note its traced need (NEED-ID or derived source)
- Note its current Quality Status field (will be updated by this skill)

### Step 3 — Apply Quality Checks

For each requirement, apply all eight quality attribute
checks. Be systematic — do not skip attributes for
requirements that appear obviously well-formed.

For each attribute check:

- **PASS** — the requirement satisfies this attribute
- **FAIL** — the requirement fails this attribute;
  record the specific failure reason
- **WARN** — the requirement is borderline; record
  the concern and recommended improvement
- **N/A** — the attribute genuinely does not apply;
  record rationale

For FAIL items, provide a specific rewrite suggestion
that would make the requirement pass. Do not just
flag the problem — fix it.

### Step 4 — Check Cross-Requirement Consistency

After checking individual requirements, check pairs
for consistency:

- Do any two requirements contradict each other?
- Do any two requirements overlap to the point of
  duplication?
- Do requirements collectively cover all stakeholder
  needs, or are there needs with no corresponding
  requirement?

For each gap — a need with no requirement — flag it
as a coverage gap requiring a new requirement.

### Step 5 — Write Output Artifacts

#### 5a — Requirements Quality Report

Write `specs/system/04-requirements/req-quality-report.md`:

```markdown
# Requirements Quality Report

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** {PASS | FAIL | PASS WITH WARNINGS}
**Last Updated By:** se-req-quality skill

---

## Summary

| Metric | Count |
| ------ | ----- |
| Total requirements checked | {N} |
| Requirements fully passing | {N} |
| Requirements with FAILs | {N} |
| Requirements with WARNs only | {N} |
| Coverage gaps (needs without requirements) | {N} |

**Overall Result:** PASS | FAIL | PASS WITH WARNINGS

{One paragraph summary. If FAIL, state the most critical
issues leading with Verifiable failures.}

---

## Results by Requirement

### {REQ-ID} — {first 8 words of requirement text}

**Full Text:** "{exact requirement text}"
**Source Need:** {NEED-ID or "Derived from REQ-ID" or "MISSING — see below"}

| Attribute | Result | Finding |
| --------- | ------ | ------- |
| Necessary | PASS/FAIL/WARN | {finding} |
| Unambiguous | PASS/FAIL/WARN | {finding} |
| Verifiable | PASS/FAIL/WARN | {finding} |
| Consistent | PASS/FAIL/WARN | {finding} |
| Complete | PASS/FAIL/WARN | {finding} |
| Singular | PASS/FAIL/WARN | {finding} |
| Feasible | PASS/FAIL/WARN | {finding} |
| Traceable | PASS/FAIL/WARN | {finding} |

{If any FAIL:}
**Rewrite Suggestion:**
"{improved requirement text that addresses all failures}"

---

## Coverage Gaps

| Need ID | Need Description | Gap | Recommended Action |
| ------- | ---------------- | --- | ------------------ |
| NEED-STK-001-003 | {need text} | No requirement addresses this need | Add requirement: {suggested text} |

---

## Consistency Issues

| Issue ID | Requirement A | Requirement B | Issue | Resolution |
| -------- | ------------- | ------------- | ----- | ---------- |

---

## Action Summary

### Must Fix (FAIL items — Phase 2 cannot close until resolved)

| REQ-ID | Attribute Failed | Action Required |
| ------ | ---------------- | --------------- |

### Should Fix (WARN items — recommended before Phase 3)

| REQ-ID | Attribute | Concern | Recommendation | Feature Introduced | Status | Acceptance Rationale |
| ------ | --------- | ------- | -------------- | ------------------ | ------ | -------------------- |
```

**Should Fix column guidance:**

- `Feature Introduced` — the feature ID currently being worked
  (e.g. `007-python312-upgrade`). Never blank — this is the
  accountability anchor for items that carry forward.
- `Status` — one of:
  - `Open` — not yet resolved; must be addressed before Phase 3
    unless explicitly accepted
  - `Resolved` — the requirement was rewritten and passes the
    attribute; update this column when fixed
  - `Accepted with Rationale` — the team has reviewed and
    consciously accepted the risk of leaving this item unfixed;
    the rationale must be recorded in `Acceptance Rationale`
- `Acceptance Rationale` — required when Status is
  `Accepted with Rationale`; blank otherwise

When re-running this skill on a subsequent feature, **do not
delete prior Should Fix rows**. Instead:

- Add new rows for any new WARN items found in this run
- Update Status of previously Open items if they have been
  resolved or accepted since the last run
- Set `Feature Introduced` to the feature in which the WARN
  was first identified (preserve it on update, do not change it)

The Phase 2 gate checks that no Should Fix row has Status =
`Open` without documented Acceptance Rationale. A row that
has been Open through multiple features without rationale will
fail the gate.

#### 5b — Update Requirements Register Quality Status

Update `specs/system/04-requirements/requirements-register.md`:

- Set the Quality Status column for each requirement to
  PASS, FAIL, or PASS WITH WARNINGS
- Update the Statistics section with final counts
- Update Last Updated By to `se-req-quality skill`

Do not remove or restructure the register — only update the
Quality Status column and statistics.

### Step 6 — Update Risk Register

**Risk ID assignment:** Before appending new entries, scan
the register for all existing `RISK-NNN` IDs and determine
the maximum N. Assign new IDs starting from `max_N + 1`.
Never derive the next ID from the last row alone — use the
maximum across the entire document to prevent collisions.
Set `Status` = `Open` and `Residual P`, `Residual C`,
`Residual Level` = `—` for all newly created entries.
Update the Summary table counts after adding entries.

Add risk entries for:

- Any requirement failing the Verifiable attribute where
  no clear fix exists
- Any coverage gap — a need with no requirement
- Any consistency conflict between requirements that
  has no clear resolution
- Any requirement marked Feasible=WARN or Feasible=FAIL

### Step 7 — Confirm Completion

Report to the user:

1. Overall quality gate result: PASS, FAIL, or
   PASS WITH WARNINGS
2. Total requirements checked
3. Count of FAIL items — list them by REQ-ID and
   failed attribute
4. Count of WARN items
5. Count of coverage gaps (needs with no requirement)
6. Count of requirements whose quality status was
   updated in the register
7. Count of risk entries added
8. If FAIL: state clearly that Phase 2 gate will not
   pass until all FAIL items are resolved
9. Recommended next skill:
   - If FAIL items exist: re-run `/thing.se-requirements`
     to fix requirements, then re-run this skill
   - If PASS or PASS WITH WARNINGS:
     `/thing.se-gate 2`
