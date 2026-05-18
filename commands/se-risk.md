---
description: "Finalise the risk register at Phase 6. The risk register has been built incrementally since Phase 0 — this skill audits every entry, ensures all risks are dispositioned, escalates any unacceptable residual risks, and produces a final risk closure report. Every risk must be Closed, Accepted, or Carried Forward with documented rationale before the Phase 6 gate can pass."
---

# SE Risk

## Purpose

Finalise and close the risk register. Risk management is a
threaded activity — the register has been growing since Phase 0
with entries from stakeholder assumptions, NFR gaps, architectural
decisions, interface unknowns, and verification results. This
skill performs the final audit, ensures every risk is
dispositioned, and produces the closure report.

Risk management follows a structured risk management process:
**Identify → Analyse → Handle → Monitor → Close**

This skill executes the Handle, Monitor, and Close steps
for all outstanding risks. Identification and Analysis have
been ongoing throughout all phases.

Output artifacts:

- `specs/system/10-risk/risk-register.md` (finalised)
- `specs/system/10-risk/risk-closure-report.md`

Updates:

- `specs/system/rtm.md` (risk disposition column
  where risks relate to requirements)

## Pre-Conditions

- `specs/system/10-risk/risk-register.md` must exist.
  Read it in full before doing anything else.
- `specs/system/09-vv/vv-results.md` must exist.
  Read it — failed verification events are risks.
- `specs/system/rtm.md` must exist. Read it.
- `.specify/memory/constitution.md` must exist. Read it.
  The regulatory context determines what risk levels
  are acceptable without formal sign-off.

If `specs/system/10-risk/risk-register.md` does not exist, stop.
Tell the user it should have been created by
`/thing.se-constitution` or
`/thing.se-stakeholders` and updated throughout all phases.

## Risk Scoring Model

Apply this model consistently for all risk entries.
Use it to recalibrate scores that were set as defaults
(P=3) in earlier phases now that more is known.

### Probability (P) — likelihood of occurrence

| Score | Label | Description |
| ----- | ----- | ----------- |
| 1 | Rare | Unlikely in normal circumstances |
| 2 | Unlikely | Could occur but not expected |
| 3 | Possible | Might occur — insufficient evidence either way |
| 4 | Likely | Will probably occur |
| 5 | Almost Certain | Expected to occur |

### Consequence (C) — impact if it occurs

| Score | Label | Description |
| ----- | ----- | ----------- |
| 1 | Negligible | No meaningful impact on system or stakeholders |
| 2 | Minor | Small impact, easily managed |
| 3 | Moderate | Noticeable impact, manageable with effort |
| 4 | Significant | Major impact on schedule, cost, quality, or safety |
| 5 | Severe | System failure, safety incident, or regulatory rejection |

### Risk Level = P × C

| Level | Range | Treatment |
| ----- | ----- | --------- |
| Low | 1–4 | Accept or monitor |
| Medium | 5–9 | Mitigate or monitor with action plan |
| High | 10–14 | Mitigate — action required before closure |
| Critical | 15–25 | Immediate action — cannot be accepted without |
| | | explicit sign-off at programme level |

## Handling Strategies

**Avoid** — change the plan to eliminate the risk entirely.
The risk source is removed.

**Mitigate** — take action to reduce probability, consequence,
or both. Produces a residual risk at a lower level.

**Transfer** — shift the risk to another party via contract,
insurance, or supplier agreement. The risk is not eliminated
— it is owned elsewhere.

**Accept** — acknowledge the risk and decide no action is
warranted. Must be explicit and documented. Requires
sign-off for Medium and above.

**Watch** — monitor the risk for change without active
handling. Only appropriate for Low risks or risks that
cannot yet be handled due to insufficient information.

## Instructions

### Step 1 — Read and Categorise All Risks

Read the risk register in full. For every entry, record:

- Current probability, consequence, and risk level
- Current handling strategy
- Current status: Open | Mitigated | Closed | Accepted |
  Carried Forward
- Which phase identified this risk
- Whether a mitigation action was defined and whether
  it was completed

Group risks by status:

- **Unhandled** — Open with no handling strategy defined
- **Mitigation in progress** — strategy defined but
  action not yet completed
- **Mitigation complete** — action completed, residual
  risk to be assessed
- **Accepted** — explicitly accepted, needs sign-off check
- **Closed** — risk no longer applicable, needs rationale check
- **Carried Forward** — will persist beyond this feature

### Step 2 — Recalibrate Risk Scores

Many early risk entries were created with default scores
(P=3) because insufficient information existed at the time.
Now that the system has been built and verified, recalibrate
every entry where P or C was a default estimate.

For each entry with P=3 (Possible — insufficient evidence):

Ask the user: *"Risk {RISK-ID}: '{description}' was scored
P=3 as a default when it was identified in Phase {N}.
Now that we have more information, has this risk
materialised, been disproven, or do we have better
evidence of its likelihood?"*

Options:

- Evidence shows it is unlikely → reduce P
- Evidence shows it is likely or has materialised → increase P
- Still insufficient evidence → keep P=3 and flag for
  Carried Forward if not resolved

### Step 3 — Handle All Unhandled Risks

For every risk with no handling strategy defined, determine
the appropriate strategy now.

Tell the user: *"The following risks have no handling
strategy defined. We need to disposition each one before
the Phase 6 gate can pass."*

For each unhandled risk, ask:

1. Is this risk still relevant? Has it been rendered
   moot by decisions made during the project?
   (If yes → Close with rationale)
2. Can the risk source be eliminated entirely?
   (If yes → Avoid, describe what changes)
3. Has action already been taken to reduce it?
   (If yes → Mitigate, describe what was done and
   assess residual risk)
4. Is this a risk that another party should own?
   (If yes → Transfer, identify the receiving party
   and the mechanism)
5. Is this a risk the project consciously accepts?
   (If yes → Accept, document rationale and obtain
   sign-off for Medium and above)
6. Does this risk persist beyond this feature and
   need ongoing monitoring?
   (If yes → Carry Forward with owner and review date)

### Step 4 — Assess Residual Risks

For every risk that has been mitigated, assess the
residual risk — the risk that remains after the
mitigation action has been applied.

For each mitigated risk:

1. What was the original risk level (P×C)?
2. What mitigation action was taken?
3. Has the mitigation action been completed and verified?
4. What is the new probability after mitigation?
5. What is the new consequence after mitigation?
6. What is the residual risk level?
7. Is the residual risk level acceptable?
   - For safety-critical systems: reference the
     applicable standard's acceptability criteria
     (e.g. ISO 14971 risk acceptability matrix,
     MIL-STD-882 risk acceptance authority levels)
   - For non-safety systems: use the risk level
     table above — residual High or Critical requires
     explicit acceptance

### Step 5 — Escalation Check

Identify any risks that require escalation before
acceptance:

**Mandatory escalation triggers:**

- Any risk with residual level Critical (15–25)
- Any risk with residual level High (10–14) that
  is being Accepted rather than mitigated further
- Any safety risk (consequence = 5) regardless of
  probability
- Any risk that affects regulatory compliance
- Any risk that has materialised (i.e. has become
  an issue, not just a risk)

For each escalation-required risk, tell the user:
*"Risk {RISK-ID} requires explicit programme-level
sign-off before it can be accepted. Who is the
appropriate authority to accept this risk?
This must be recorded in the risk register."*

### Step 6 — Risks from Failed Verification Events

Read `specs/system/09-vv/vv-results.md`. For every verification event
with a result of Fail:

1. Does a risk entry already exist for this failure?
   If not, create one now.
2. What is the consequence of this failed verification
   event on the system's fitness for purpose?
3. Has a defect been raised and is it being tracked?
4. What is the disposition — will it be fixed before
   deployment, accepted as a known limitation, or
   does it block deployment?

A failed verification event that is not a risk entry
is an unmanaged issue. It must be entered into the
risk register and dispositioned.

### Step 7 — Identify Risks to Carry Forward

Some risks are not closeable at this phase — they
persist into operation, maintenance, or future
development. These must be explicitly carried forward
rather than silently left open.

For each risk that cannot be closed:

1. Why can it not be closed now?
2. Who owns it going forward?
3. What is the review cadence? (monthly, per release,
   on specific trigger events)
4. What would trigger escalation of this risk in future?
5. Where will it be tracked after this project closes?
   (operational risk register, programme risk log,
   product backlog)

### Step 8 — Write Finalised Risk Register

Rewrite `specs/system/10-risk/risk-register.md`
with complete, audited, dispositioned content:

```markdown
# Risk Register

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** {OPEN | FINALISED | FINALISED WITH CARRIED FORWARD ITEMS}
**Last Updated By:** se-risk skill
**Version:** {increment from previous}

---

## Summary

| Status | Count |
| ------ | ----- |
| Closed | {N} |
| Accepted | {N} |
| Carried Forward | {N} |
| Open (unresolved) | {N} |
| **Total** | **{N}** |

**Critical risks outstanding:** {N}
**High risks outstanding:** {N}
**Escalation required:** {yes — see escalation section | no}

---

## Risk Register

| Risk ID | Description | Category | Phase Identified | P | C | Level | Strategy | Mitigation Action | Residual P | Residual C | Residual Level | Status | Owner | Notes |
| ------- | ----------- | -------- | ---------------- | - | - | ----- | -------- | ----------------- | ---------- | ---------- | -------------- | ------ | ----- | ----- |
| RISK-001 | {description} | {category} | {phase} | {1-5} | {1-5} | {P×C} | {strategy} | {action taken} | {1-5} | {1-5} | {P×C} | {status} | {owner} | |

---

## Risk Categories Used

| Category | Description |
| -------- | ----------- |
| Assumption | An unvalidated belief that was made during planning |
| Technical | A technical uncertainty or challenge |
| Interface | A risk at a system or subsystem boundary |
| Regulatory | A risk related to compliance or certification |
| Schedule | A risk to delivery timeline |
| Safety | A risk to the safety of persons, property, or environment |
| Stakeholder | A risk related to stakeholder availability or acceptance |
| External | A risk from outside the project's control |

---

## Escalation Register

{Risks requiring programme-level acceptance sign-off}

| Risk ID | Risk Level | Escalation Reason | Authority | Sign-off Status | Date |
| ------- | ---------- | ----------------- | --------- | --------------- | ---- |

---

## Carried Forward Risks

{Risks that persist beyond this feature into operation
or future development}

| Risk ID | Description | Residual Level | Owner | Review Cadence | Tracking Location |
| ------- | ----------- | -------------- | ----- | -------------- | ----------------- |

---

## Closed Risks

{Risks that are no longer applicable — retained for
audit trail}

| Risk ID | Description | Original Level | Closure Rationale | Closed Date |
| ------- | ----------- | -------------- | ----------------- | ----------- |
```

### Step 9 — Write Risk Closure Report

Write `specs/system/10-risk/risk-closure-report.md`:

```markdown
# Risk Closure Report

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Prepared By:** se-risk skill
**Status:** {CLOSED | CLOSED WITH ACCEPTED RISKS |
CLOSED WITH CARRIED FORWARD ITEMS}

---

## Executive Summary

{Two to three paragraphs. State the overall risk
disposition status. How many risks were identified
across all phases, how many were mitigated, how many
accepted, how many carried forward, and whether any
Critical or High risks remain outstanding. Written
for a programme manager, customer, or regulator.}

---

## Risk Journey by Phase

| Phase | Risks Identified | Source |
| ----- | ---------------- | ------ |
| Phase 0 — Problem Space | {N} | Stakeholder assumptions, ConOps gaps |
| Phase 1 — Solution Space | {N} | NFR gaps, exploration unknowns |
| Phase 2 — Requirements | {N} | Requirements quality failures, coverage gaps |
| Phase 3 — Architecture | {N} | Architectural decisions, interface risks |
| Phase 4 — Implementation Planning | {N} | Task coverage gaps |
| Phase 5 — Implementation | {N} | Implementation issues |
| Phase 6 — V&V | {N} | Failed verification events |
| **Total** | **{N}** | |

---

## Disposition Summary

| Strategy | Count | Notes |
| -------- | ----- | ----- |
| Avoided | {N} | Risk source eliminated |
| Mitigated | {N} | Action taken, residual risk accepted |
| Transferred | {N} | Risk owned by external party |
| Accepted | {N} | Conscious acceptance with rationale |
| Carried Forward | {N} | Persists into operation or future work |
| Closed (no longer applicable) | {N} | |
| **Total** | **{N}** | |

---

## Residual Risk Profile

| Level | Count | Acceptable Without Escalation |
| ----- | ----- | ----------------------------- |
| Critical (15–25) | {N} | No — requires programme sign-off |
| High (10–14) | {N} | No — requires explicit acceptance |
| Medium (5–9) | {N} | Yes — with documented rationale |
| Low (1–4) | {N} | Yes |

---

## Accepted Risks

{All risks accepted without full mitigation.
Each must have documented rationale.}

| Risk ID | Description | Residual Level | Acceptance Rationale | Accepted By |
| ------- | ----------- | -------------- | -------------------- | ----------- |

---

## Carried Forward Risks

| Risk ID | Description | Residual Level | Owner | Review Cadence |
| ------- | ----------- | -------------- | ----- | -------------- |

---

## Conclusion

{One paragraph. State whether the risk profile is
acceptable for the system to proceed to deployment
or the next phase. Name any conditions — e.g. "the
system may proceed to deployment subject to resolution
of RISK-047 before first clinical use" — or state
clearly that all risks have been dispositioned to
an acceptable level.}
```

### Step 10 — Update RTM

For any requirement whose implementation or verification
is affected by an accepted or carried-forward risk,
add a note in the `specs/system/rtm.md` Gaps column
referencing the RISK-ID.

This ensures the RTM and risk register are cross-linked
— an auditor following either document can find the
other.

### Step 11 — Confirm Completion

Report to the user:

1. Total risks in the register
2. Disposition breakdown: Closed / Mitigated /
   Accepted / Transferred / Carried Forward / Open
3. Critical risks outstanding — if any, state clearly
   that Phase 6 gate cannot pass until they are
   escalated and accepted
4. High risks outstanding — same requirement
5. Number of risks carried forward with owners
   and review cadences
6. Whether the risk register status is FINALISED
   or FINALISED WITH CARRIED FORWARD ITEMS
7. Recommended next skill:
   `/thing.se-gate 6`
   (final phase gate — the last step before closure)
