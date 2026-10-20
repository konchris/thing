---
description: "Draft the Concept of Operations (ConOps) for the system. Describes how the system will be used in the operational environment, by whom, under what conditions, and in what modes. Written entirely in the operational domain — no architecture or implementation decisions. Produces conops.md and operational-scenarios.md."
---

# SE ConOps

## Purpose

Produce a Concept of Operations (ConOps) document that describes
the system from the perspective of those who will use and operate
it. The ConOps is written in the language of the operational domain,
not the engineering domain. It contains no architecture decisions,
no technology choices, and no implementation details.

The ConOps answers: *What will this system do for people, in what
situations, and under what conditions?*

Output artifacts:

- `specs/system/01-conops/conops.md`
- `specs/system/01-conops/operational-scenarios.md`
- `specs/features/$FEATURE_ID/01-conops/conops-delta.md` (feature delta)

Updates:

- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it.
- `specs/system/00-stakeholders/stakeholder-register.md`
  must exist. Read it.
- `specs/system/00-stakeholders/needs-register.md`
  must exist. Read it.
- `specs/system/00-stakeholders/personas/` must contain
  at least one persona. Read all of them.

If any pre-condition is not met, stop and tell the user which
skill must be run first.

## Baseline-Update Mode

The system-level artifacts this skill writes are a baseline: one
current description of the whole system, not a log of features.
This applies to:

- `specs/system/01-conops/conops.md`
- `specs/system/01-conops/operational-scenarios.md`

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
2. Modes, success criteria, open issues and the scenario index
   each live in one table for the whole system. Each scenario has
   one section in `operational-scenarios.md`; a changed scenario
   is edited in that section.
3. Add new rows to the existing table. Edit changed rows in place;
   where the history matters, add
   "revised {YYYY-MM-DD}, {feature ID}" to the row's notes.
4. Retire rows that are no longer true: strike through the text
   (`~~…~~`), mark it `Retired` after its name (for a scenario, in
   the index and at the top of its section), and keep the row so
   its ID stays reserved. Record the reason in the feature delta,
   not in the baseline.
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
- `specs/system/10-risk/risk-register.md` (threaded; this skill updates it)

Correct every statement in them that the feature has made
untrue, including in artifacts that earlier skills own.
Re-assess every risk, V&V entry or RTM row in the artifacts
above whose mitigation or subject the feature removes or
changes.

Do not search or edit the artifacts of later development steps
(`02-nfr/`, `04-requirements/`, `05-architecture/`,
`06-interfaces/`, `09-vv/` under `specs/system/`). Their skills
bring them up to date when they run for this feature, and each
phase gate checks them.

Finally, write the feature delta
`specs/features/$FEATURE_ID/01-conops/conops-delta.md` using the
template at `artifacts/baseline-delta.md`. If it already exists
(this skill ran earlier for the same feature), append to its
tables. Record:

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

### Step 1 — Read Context

Read all pre-condition artifacts. Build an internal summary of:

- The system's purpose and operational environment
- All stakeholders and their primary needs
- Any conflicts recorded in the needs register
- The regulatory and safety context from the constitution

Do not ask the user to repeat information already captured.
Reference it directly.

### Step 2 — Operational Context

Tell the user: *"We'll now describe how this system fits into
the operational world — the environment it lives in, the
conditions it operates under, and how it relates to other
systems and processes around it."*

Ask the following, one at a time:

1. Where is this system physically or logically deployed?
   (e.g. hospital ward, factory floor, cloud infrastructure,
   embedded in a device, installed on a vehicle)
2. What are the normal operating conditions?
   (e.g. 24/7 continuous, business hours only, event-triggered,
   batch processing)
3. What are the abnormal or degraded operating conditions the
   system must handle?
   (e.g. network loss, power failure, operator error, peak load,
   emergency situations)
4. What external systems, people, or processes does this system
   depend on as inputs?
5. What external systems, people, or processes depend on this
   system's outputs?
6. Are there periods when the system must not be unavailable?
   (planned maintenance windows, regulatory blackout periods,
   critical operational phases)

### Step 3 — Modes of Operation

Tell the user: *"Most systems operate in more than one mode —
for example, normal operation, startup, shutdown, maintenance,
emergency, or degraded operation. Let's identify the modes
for this system."*

1. What modes of operation does this system have?
   (Prompt with examples if the user is unsure: normal, startup,
   shutdown, standby, maintenance, degraded, emergency, calibration)
2. For each mode: what triggers entry into that mode, what
   happens in that mode, and what triggers exit from it?
3. Are there any modes where safety behaviour changes?
   (e.g. reduced functionality, mandatory alerts, operator
   intervention required)

Document each mode as a named entry. Assign a Mode ID: `MODE-{NNN}`.

### Step 4 — Operational Scenarios

Tell the user: *"Now let's walk through the key situations this
system will face in operation. We call these operational scenarios.
Each one describes a real-world situation from start to finish,
from the perspective of the people involved — not the system's
internal behaviour."*

Identify at minimum the following scenario types. Ask the user
to describe each one, prompting with questions if they are unsure:

- **Normal success scenario** — the most common, expected use
  of the system working as intended
- **Alternative success scenario** — a valid but less common
  path that also results in success
- **Operator error scenario** — what happens when a user makes
  a predictable mistake
- **System failure scenario** — what happens when the system
  itself fails or behaves incorrectly
- **Degraded operation scenario** — what happens when the system
  operates with reduced capability (e.g. one component unavailable)
- **Emergency or safety scenario** — if applicable given the
  safety classification in the constitution

For domains with regulatory requirements (medical, aerospace,
defence, automotive), also capture:

- **Misuse scenario** — foreseeable misuse that the system must
  detect or prevent
- **Maintenance scenario** — how a maintainer interacts with
  the system during servicing

For each scenario assign a Scenario ID: `SCN-{NNN}`.

Ask for each scenario:

1. What triggers this scenario? (precondition)
2. Who is involved and what is their goal?
3. What happens step by step? (in operational terms — actions
   and events, not system internals)
4. What is the successful outcome?
5. What could go wrong within this scenario?
6. What are the consequences if this scenario ends badly?

### Step 5 — Success Criteria

Tell the user: *"Finally, let's define what success looks like
for this system in operation — the measurable conditions that
would tell us the system is delivering value."*

1. How will the primary users know the system is working correctly?
2. How will the system owners or sponsors measure success?
3. Are there any operational thresholds that must never be
   violated? (e.g. maximum response time, minimum availability,
   error rate limits)
4. What would cause stakeholders to declare the system a failure,
   even if it is technically functional?

### Step 6 — Write Output Artifacts

#### 6a — ConOps Document

Write `specs/system/01-conops/conops.md`, or merge into it if it
exists (see Baseline-Update Mode).
The template shows the whole document; when merging, the
feature's modes, success criteria and open issues go into the
existing §4, §5 and §6 tables:

```markdown
# Concept of Operations

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** DRAFT
**Last Updated By:** se-conops skill

---

## 1. System Overview

{Two to three paragraphs describing the system's purpose,
the problem it solves, and its place in the operational
environment. Written for a non-technical operational audience.
No architecture or technology.}

## 2. Operational Environment

**Deployment Context:** {where and how the system is deployed}
**Normal Operating Conditions:** {description}
**Abnormal/Degraded Conditions:** {description}

### External Dependencies (Inputs)
{What this system depends on from the outside world}

### External Dependents (Outputs)
{What depends on this system's outputs}

### Operational Constraints
{Availability windows, blackout periods, maintenance windows}

## 3. Stakeholders and Roles

{Brief summary of each stakeholder's operational role.
Reference the stakeholder register — do not duplicate it.}

## 4. Modes of Operation

| Mode ID | Mode Name | Entry Trigger | Description | Exit Trigger | Safety Notes |
| ------- | --------- | ------------- | ----------- | ------------ | ------------ |
| MODE-001 | Normal Operation | System startup complete | {description} | Shutdown command | None |

## 5. Success Criteria

| Criterion ID | Description | Measurable Threshold | Stakeholder |
| ------------ | ----------- | -------------------- | ----------- |
| SC-001 | {what success looks like} | {how measured} | {who cares} |

## 6. Open Issues

| ID | Issue | Owner | Target Resolution |
| -- | ----- | ----- | ----------------- |

---

*This document describes the operational concept only.
Architecture, design, and implementation decisions are
out of scope for this document.*
```

#### 6b — Operational Scenarios

Write `specs/system/01-conops/operational-scenarios.md`, or merge
into it if it exists (see Baseline-Update Mode):

```markdown
# Operational Scenarios

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** DRAFT
**Last Updated By:** se-conops skill

---

## Scenario Index

| ID | Name | Type | Primary Stakeholder | Priority |
| -- | ---- | ---- | ------------------- | -------- |

---

## Scenarios

### {SCN-001} — {Scenario Name}

**Type:** {Normal Success | Alternative Success | Operator Error |
System Failure | Degraded Operation | Emergency | Misuse | Maintenance}
**Primary Stakeholder:** {STK-NNN}
**Precondition:** {what must be true for this scenario to begin}

**Scenario Steps:**

1. {operational action or event — stakeholder or environment perspective}
2. {next step}
...

**Successful Outcome:** {what the world looks like when this ends well}

**Failure Modes Within This Scenario:**
- {what could go wrong at step N}

**Consequences of Failure:** {operational impact}

**Notes:** {anything relevant — regulatory obligations, safety
considerations, links to needs register entries}

---
```

### Step 7 — Update Risk Register

Open `specs/system/10-risk/risk-register.md`.

Add risk entries for:

- Any abnormal operating condition identified in Step 2 that
  has no defined system response yet
- Any mode transition identified in Step 3 with undefined or
  unclear behaviour
- Any scenario failure mode identified in Step 4 with high
  consequence
- Any success criterion in Step 5 that appears technically
  challenging or unverified

### Step 8 — Confirm Completion

Report to the user:

1. Number of modes of operation defined
2. Number of operational scenarios documented
3. Number of success criteria defined
4. Number of risk entries added
5. Any open issues recorded in the ConOps
6. Reminder: conflicts from the needs register that remain
   unresolved should be addressed through the scenarios —
   flag any that are still open
7. Recommended next skill: `/thing.se-nfr`

Also report the feature delta path, the number of baseline
items added, changed and retired, and every statement corrected
in an artifact this skill does not own.
