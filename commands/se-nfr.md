---
description: "Elicit and classify non-functional requirements (quality attributes) following ISO/IEC 25010. Captures performance, reliability, security, safety, maintainability, and other system quality characteristics. Seeds the V&V plan with an entry for each NFR. Updates the risk register with quality-related risks."
---

# SE Non-Functional Requirements

## Purpose

Capture the quality attributes and constraints that define how
the system must perform its functions — not what functions it
performs. These are the non-functional requirements (NFRs),
also called system quality attributes per ISO/IEC 25010.

NFRs are first-class requirements. They are verifiable, traceable,
and must be satisfied alongside functional requirements. Many
architectural decisions are driven primarily by NFRs, not by
functional requirements.

This skill initiates the V&V plan with an entry for each NFR,
because a quality attribute without a verification method is
not a requirement — it is a wish.

Output artifacts:

- `specs/system/02-nfr/quality-attributes.md`

Initiates:

- `specs/system/09-vv/vv-plan.md` (created with NFR entries)

Updates:

- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it.
  Note the priority quality attributes and regulatory context.
- `specs/system/01-conops/conops.md` must exist. Read it.
  The operational environment, modes, and scenarios will
  directly inform which quality attributes matter most.
- `specs/system/00-stakeholders/needs-register.md`
  must exist. Read it. Some NFRs will be directly traceable
  to stakeholder needs.

If any pre-condition is not met, stop and tell the user which
skill must be run first.

## Instructions

### Step 1 — Read Context

Read all pre-condition artifacts. Build an internal picture of:

- Priority quality attributes already named in the constitution
- Operational conditions that stress quality attributes
  (e.g. 24/7 operation → availability; clinical environment →
  safety and security; real-time control → performance)
- Stakeholder needs that imply quality attributes
  (e.g. "must not lose data" → reliability + data integrity)
- Regulatory standards that mandate specific quality attributes
  (e.g. IEC 62304 → maintainability; ISO 14971 → safety;
  IEC 62443 → security)

Do not ask the user to repeat information already captured.
Reference it and build on it.

### Step 2 — Introduce the ISO/IEC 25010 Quality Model

Tell the user: *"We'll now capture the non-functional requirements
for your system — the qualities that define how well it must
perform, how safe and secure it must be, how easy to maintain,
and so on. I'll work through each quality category systematically.
For each one, tell me what matters for your system and I'll help
you express it as a verifiable requirement."*

Work through each category below. For each one:

- Briefly explain what it covers in plain language
- Reference any relevant context from the ConOps or constitution
- Ask the targeted questions
- For each NFR identified, assign an ID and capture it

NFR ID format: `NFR-{CATEGORY CODE}-{NNN}`
Category codes: PERF · REL · SEC · SAF · USE · MAIN · PORT · INT · REG

---

### Step 3 — Elicit NFRs by Category

Work through each category conversationally. Skip a category
only if the user explicitly confirms it is not applicable —
do not skip silently.

#### Category: Performance (PERF)

Covers response time, throughput, resource utilisation, capacity.

Ask:

1. Are there response time requirements? What must happen within
   what time under what load conditions?
2. Are there throughput requirements? (transactions per second,
   messages per minute, data volume per hour)
3. Are there resource constraints? (CPU, memory, storage, network
   bandwidth limits)
4. What is the expected peak load and how does behaviour change
   under peak conditions?

#### Category: Reliability (REL)

Covers availability, fault tolerance, recoverability, maturity.

Ask:

1. What is the required availability? (e.g. 99.9% uptime,
   maximum N hours downtime per month)
2. What happens when a component fails — must the system
   degrade gracefully or fail safe?
3. What is the acceptable mean time between failures (MTBF)?
4. What is the maximum acceptable recovery time after a failure
   (RTO) and maximum acceptable data loss (RPO)?
5. Are there redundancy requirements?

#### Category: Security (SEC)

Covers confidentiality, integrity, non-repudiation,
authenticity, accountability.

Ask:

1. What data does this system handle and what is its
   sensitivity classification?
2. Who must be prevented from accessing the system or its data?
3. Are there authentication and authorisation requirements?
4. Must the system maintain an audit trail of actions?
5. Are there applicable security standards or frameworks?
   (e.g. IEC 62443, NIST, ISO 27001, FDA cybersecurity guidance,
   HIPAA, GDPR)
6. Are there data retention or data deletion requirements?

#### Category: Safety (SAF)

Covers freedom from unacceptable risk of harm to persons,
property, or environment. Ask this section only if the
constitution indicates the system is safety-critical or
safety-related.

Ask:

1. What is the safety integrity level or software safety class?
   (e.g. IEC 62304 Class A/B/C, IEC 61508 SIL, ISO 26262 ASIL)
2. What are the safety functions this system must perform?
3. What must happen if a safety function fails?
   (fail safe, fail operational, alert and hand off)
4. Are there watchdog, self-test, or diagnostic requirements?
5. Are there requirements to detect and respond to hazardous
   inputs or states?

#### Category: Usability (USE)

Covers learnability, operability, accessibility, user error
protection.

Ask:

1. What is the expected training time for a new operator?
2. Are there accessibility requirements? (visual, motor,
   cognitive — regulatory or otherwise)
3. What is the acceptable error rate for trained operators?
4. Must the system protect against or recover from operator
   errors without data loss?
5. Are there requirements for feedback, confirmation dialogs,
   or undo capability?

#### Category: Maintainability (MAIN)

Covers modularity, analysability, modifiability, testability.

Ask:

1. Who will maintain this system and what is their technical
   level?
2. How frequently is the system expected to be updated?
3. Must individual components be replaceable without system
   downtime?
4. Are there requirements for logging, diagnostics, or
   observability to support fault analysis?
5. Are there requirements for automated testing coverage?
   (e.g. minimum unit test coverage percentage)

#### Category: Portability (PORT)

Covers adaptability, installability, replaceability.

Ask:

1. Must the system run on multiple platforms, operating systems,
   or hardware configurations?
2. Are there installation or deployment constraints?
   (e.g. no internet access during install, air-gapped environment)
3. Must the system be replaceable by an alternative in future
   without data loss?

#### Category: Interoperability (INT)

Covers the ability to exchange and use information with other
systems.

Ask:

1. What external systems must this system communicate with?
2. Are there mandated protocols, data formats, or interface
   standards?
3. Are there existing systems whose interfaces cannot be changed
   that this system must conform to?

#### Category: Regulatory Compliance (REG)

Covers requirements imposed by law, regulation, or certification.

Ask:

1. Are there specific clauses in applicable standards that
   impose measurable system requirements?
   (Work through each standard named in the constitution)
2. Are there documentation or record-keeping requirements
   imposed by regulation?
3. Are there traceability requirements imposed by regulation?
   (e.g. IEC 62304 requires software requirements to be traced
   to software items)

---

### Step 4 — Consolidate and Prioritise

After working through all categories:

1. Review the full list of NFRs captured with the user.
2. Identify any NFRs that appear to conflict with each other.
   (e.g. maximum security vs. maximum usability;
   maximum performance vs. minimum resource use)
3. For each conflict, ask the user to state which takes
   precedence and under what conditions.
4. Assign a priority to each NFR: Must Have · Should Have ·
   Nice to Have (MoSCoW)

### Step 5 — Write Output Artifacts

#### 5a — Quality Attributes Document

Write `specs/system/02-nfr/quality-attributes.md`:

```markdown
# Quality Attributes and Non-Functional Requirements

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** DRAFT
**Last Updated By:** se-nfr skill

---

## Summary

| NFR ID | Category | Short Description | Priority | Verification Method | Status |
| ------ | -------- | ----------------- | -------- | ------------------- | ------ |
| NFR-PERF-001 | Performance | {short desc} | Must Have | Test | Open |

---

## NFR Details

### {NFR-ID} — {Short Title}

**Category:** {ISO 25010 category}
**Priority:** {Must Have | Should Have | Nice to Have}
**Source:** {NEED-ID or regulatory clause or constitution}
**Status:** Open

**Statement:**
{The system shall {quality attribute} {measurable criterion}
under {conditions}.}

**Rationale:**
{Why this NFR exists — operational or regulatory justification}

**Verification Method:** {Test | Analysis | Inspection | Demonstration}
**Verification Notes:** {How compliance will be established —
what will be measured, under what conditions, with what tooling}

**Conflicts With:** {NFR-ID or None}
**Conflict Resolution:** {which takes precedence and why}

---
```

#### 5b — Seed the V&V Plan

Create `specs/system/09-vv/vv-plan.md` if it does not exist.

Write the header and add one row per NFR to the plan table:

```markdown
# Verification and Validation Plan

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** DRAFT — initiated by se-nfr, updated continuously
**Last Updated By:** se-nfr skill

---

## Purpose

This plan defines how every requirement — functional and
non-functional — will be verified or validated. It is a
threaded artifact: initiated here with NFR entries, extended
in Phase 2 with functional requirement entries, and updated
through Phase 6 when results are recorded.

A requirement without a verification method is incomplete
by definition.

---

## Verification Methods

| Code | Method | Description |
| ---- | ------ | ----------- |
| T | Test | Execute the system or component under defined conditions and observe results |
| A | Analysis | Mathematical, logical, or simulation-based evaluation without executing the system |
| I | Inspection | Visual or documented review of artifacts, code, or configuration |
| D | Demonstration | Operate the system to show a capability without formal measurement |

## Verification Stages

| Code | Stage | Description |
| ---- | ----- | ----------- |
| UT | Unit Test | Individual component or module |
| IT | Integration Test | Interaction between components |
| ST | System Test | Full system against requirements |
| AT | Acceptance Test | Stakeholder validation in operational context |

---

## V&V Plan Table

| Req ID | Type | Short Description | Method | Stage | Pass Criterion | Responsible | Feature | Status | Notes |
| ------ | ---- | ----------------- | ------ | ----- | -------------- | ----------- | ------- | ------ | ----- |
| NFR-PERF-001 | NFR | {desc} | T | ST | {measurable pass criterion} | TBD | {feature-id} | Planned | |

---

## Validation Scenarios

{Populated in Phase 6. Maps operational scenarios from ConOps
to acceptance test events.}

---

## Results

{Populated during Phase 6 execution.}
```

### Step 6 — Update Risk Register

Open `specs/system/10-risk/risk-register.md`.

**Risk ID assignment:** Before appending new entries, scan
the register for all existing `RISK-NNN` IDs and determine
the maximum N. Assign new IDs starting from `max_N + 1`.
If the register has no entries yet, start at `RISK-001`.
Never derive the next ID from the last row alone — use the
maximum across the entire document to prevent collisions.
Set `Status` = `Open` and `Residual P`, `Residual C`,
`Residual Level` = `—` for all newly created entries.
Update the Summary table counts after adding entries.

Add risk entries for:

- Any NFR with priority Must Have where the verification method
  is unclear or technically challenging
- Any NFR that conflicts with another and where the resolution
  is a priority judgment rather than a technical solution
- Any regulatory NFR where the applicable clause is not yet
  fully understood
- Any NFR where the measurable criterion is TBD

### Step 7 — Confirm Completion

Report to the user:

1. Total NFRs captured, broken down by category
2. Number of Must Have vs. Should Have vs. Nice to Have
3. Number of conflicts identified and resolution status
4. Number of V&V plan entries created
5. Number of risk entries added
6. Any NFRs with TBD verification methods — these must be
   resolved before Phase 2 gate
7. Recommended next skill: `/thing.se-explore`
