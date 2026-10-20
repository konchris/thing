---
description: "Conduct structured stakeholder elicitation following a structured stakeholder needs definition process. Produces stakeholder register, persona cards, empathy maps, and a needs register. Seeds the risk register with assumptions identified during elicitation."
---

# SE Stakeholders

## Purpose

Identify all stakeholders for the system, understand their contexts
and goals, and capture their needs in their own language — before
any solution thinking begins. This skill follows the
Stakeholder Needs and Requirements Definition process.

Output artifacts:

- `specs/system/00-stakeholders/stakeholder-register.md`
- `specs/system/00-stakeholders/personas/{stakeholder-id}.md`
- `specs/system/00-stakeholders/empathy-maps/{stakeholder-id}.md`
- `specs/system/00-stakeholders/needs-register.md`
- `specs/system/10-risk/risk-register.md` (updated with elicitation assumptions)
- `specs/features/$FEATURE_ID/00-stakeholders/stakeholders-delta.md` (feature delta)

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it before
  beginning. The system identity, domain, regulatory context, and
  canonical artifact paths are defined there.

System artifacts always live at `specs/system/` regardless of
which feature is currently being worked. The constitution's
Artifact Structure section is the authoritative path reference.

## Baseline-Update Mode

The system-level artifacts this skill writes are a baseline: one
current description of the whole system, not a log of features.
This applies to:

- `specs/system/00-stakeholders/stakeholder-register.md`
- `specs/system/00-stakeholders/needs-register.md`
- `specs/system/00-stakeholders/personas/{stakeholder-id}.md`
- `specs/system/00-stakeholders/empathy-maps/{stakeholder-id}.md`

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
3. Stakeholders, needs and conflicts each live in one table for
   the whole system. Personas and empathy maps are one file per
   stakeholder; edit the existing file rather than writing a
   second one.
4. Add new rows to the existing table. Edit changed rows in place;
   where the history matters, add
   "revised {YYYY-MM-DD}, {feature ID}" to the row's notes.
5. Retire rows that are no longer true: strike through the text
   (`~~…~~`), mark it `Retired` in its status column, or after its
   name where the table has none, and keep the row so its ID stays
   reserved. Record the reason in the feature delta, not in the
   baseline.
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
- `specs/system/10-risk/risk-register.md` (threaded; this skill updates it)

Correct every statement in them that the feature has made
untrue, including in artifacts that earlier skills own.
Re-assess every risk, V&V entry or RTM row in the artifacts
above whose mitigation or subject the feature removes or
changes.

Do not search or edit the artifacts of later development steps
(`01-conops/`, `02-nfr/`, `04-requirements/`, `05-architecture/`,
`06-interfaces/`, `09-vv/` under `specs/system/`). Their skills
bring them up to date when they run for this feature, and each
phase gate checks them.

Finally, write the feature delta
`specs/features/$FEATURE_ID/00-stakeholders/stakeholders-delta.md`
using the template at `artifacts/baseline-delta.md`. If it already
exists (this skill ran earlier for the same feature), append to
its tables. Record:

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

### Step 1 — Read Context

Read `.specify/memory/constitution.md`. Note:

- The system name and problem statement
- The industry domain and regulatory context
- The safety classification
- Any stakeholders already named in the constitution

Greet the user and briefly state what this skill will produce
and approximately how long the conversation will take
(typically 15–30 minutes for a first elicitation session).

### Step 2 — Stakeholder Identification

Tell the user: *"We'll start by identifying everyone who has an
interest in this system — not just the people who use it directly,
but anyone affected by it, responsible for it, or who can influence
its success."*

Ask the following questions conversationally, one at a time:

1. Who will directly operate or interact with the system day-to-day?
2. Who receives the outputs or depends on the results of this system?
3. Who is responsible for procuring, funding, or approving this system?
4. Who will install, maintain, or support this system after deployment?
5. Who is responsible for decommissioning or disposing of this system?
6. Are there regulatory bodies, certification authorities, or auditors
   who will evaluate this system?
7. Are there any stakeholders who could be harmed if the system
   fails or behaves incorrectly?
8. Are there any stakeholders who are not present in this conversation
   but whose needs we should represent?

For each stakeholder identified, assign:

- A short ID (e.g. `STK-001`)
- A role name (e.g. "Clinical Operator", "Maintenance Engineer")
- An interest category: User | Owner | Regulator | Maintainer |
  Affected Party | Other

### Step 3 — Persona Development

For each stakeholder identified in Step 2, ask the following.
Work through one stakeholder at a time — do not ask about all
stakeholders simultaneously.

Tell the user: *"Now let's build a profile for each stakeholder
so we understand their context deeply."*

For each stakeholder:

1. What is their technical background or expertise level with
   systems like this?
2. What does a typical working day look like for them in relation
   to this system?
3. What are their primary goals — what does success look like
   for them personally?
4. What frustrations or pain points do they currently experience
   that this system should address?
5. What would make them resistant to or suspicious of this system?
6. What constraints do they operate under? (time, environment,
   regulation, tooling)
7. How do they currently accomplish the task this system will
   support — what is their workaround today?

### Step 4 — Empathy Mapping

For each stakeholder, synthesise the answers from Step 3 into
an empathy map. You may do this without asking further questions —
infer from what has already been said. If critical information
is missing for a quadrant, ask one targeted question to fill it.

The four quadrants are:

- **Think & Feel** — what matters to them, their worries and
  aspirations
- **Hear** — what colleagues, managers, or the industry tells them
- **See** — what they observe in their environment
- **Say & Do** — what they say publicly and how they behave

Add a fifth element:

- **Jobs to Be Done** — the functional, emotional, and social
  jobs this stakeholder is hiring the system to do
  (Clayton Christensen JTBD framing)

### Step 5 — Needs Elicitation

Tell the user: *"Now we'll capture what each stakeholder actually
needs from the system — in their language, without describing
solutions. A need describes a desired outcome, not a feature."*

For each stakeholder, ask:

1. What outcomes must this system reliably deliver for you?
2. What must this system never do or allow to happen?
3. What would cause you to reject or stop using this system?
4. If this system worked perfectly, how would your work be
   different six months from now?

For each need captured:

- Assign a Need ID: `NEED-{STK-ID}-{NNN}` (e.g. `NEED-STK-001-003`)
- Record it in the stakeholder's own words first
- Then write a normalised form: action verb + object + condition
  (e.g. "The system shall notify the operator within 5 seconds
  of a sensor fault" — but without the "shall" — needs are not
  yet requirements)
- Classify: Functional | Quality | Constraint | Interface

### Step 6 — Conflicts and Priorities

1. Are there any needs from different stakeholders that appear
   to conflict with each other?
2. If resources were limited, which stakeholder needs are
   non-negotiable versus desirable?
3. Are there any needs that surprise you — things you hadn't
   expected to hear?

Document any conflicts identified for resolution in `/se-conops`.

### Step 7 — Write Output Artifacts

#### 7a — Stakeholder Register

Write `specs/system/00-stakeholders/stakeholder-register.md`, or
merge into it if it exists (see Baseline-Update Mode):

```markdown
# Stakeholder Register

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** DRAFT
**Last Updated By:** se-stakeholders skill

---

## Register

| ID | Role | Interest Category | Represented By | Priority | Notes |
| -- | ---- | ----------------- | -------------- | -------- | ----- |
| STK-001 | {role} | {category} | {person or TBD} | High/Med/Low | |

---

## Unrepresented Stakeholders

{List any stakeholders identified in Step 2 question 8 whose
needs must be inferred or researched rather than elicited directly}
```

#### 7b — Persona Cards

For each new stakeholder, write
`specs/system/00-stakeholders/personas/{stakeholder-id}.md`.
For an existing stakeholder, edit their persona in place:

```markdown
# Persona: {Role Name}

**Stakeholder ID:** {STK-NNN}
**Interest Category:** {category}
**Date:** {date}

---

## Profile

**Technical Background:** {level and relevant experience}
**Typical Interaction with System:** {frequency, context, duration}
**Operating Constraints:** {environment, time, tools, regulations}

## Goals

{What success looks like for this stakeholder — 3–5 bullet points}

## Pain Points

{Current frustrations and problems — 3–5 bullet points}

## Resistance Factors

{What would make them reject or distrust this system}

## Current Workaround

{How they accomplish this task today without the system}
```

#### 7c — Empathy Maps

For each new stakeholder, write
`specs/system/00-stakeholders/empathy-maps/{stakeholder-id}.md`.
For an existing stakeholder, edit their empathy map in place:

```markdown
# Empathy Map: {Role Name}

**Stakeholder ID:** {STK-NNN}
**Date:** {date}

---

## Think & Feel
{What matters to them deeply. Their worries, aspirations,
what keeps them up at night.}

## Hear
{What colleagues, managers, regulators, or the industry
tells them. Influences on their thinking.}

## See
{What they observe in their work environment. Examples,
competitors, problems they witness.}

## Say & Do
{What they say publicly about their work. How they actually
behave — actions, habits, contradictions between words and deeds.}

## Jobs to Be Done

| Job Type | Job Statement |
| -------- | ------------- |
| Functional | {verb} + {object} + {context} |
| Emotional | Feel {emotion} when {situation} |
| Social | Be seen as {perception} by {audience} |

---

## Summary

{Two or three sentences synthesising the most important insight
about this stakeholder for the design team.}
```

#### 7d — Needs Register

Write `specs/system/00-stakeholders/needs-register.md`, or merge
into it if it exists (see Baseline-Update Mode):

```markdown
# Needs Register

**System:** {system name}
**Baseline as of:** {feature ID}, {date}
**Feature deltas:** `specs/features/*/**/*-delta.md`
**Status:** DRAFT
**Last Updated By:** se-stakeholders skill

---

## Needs

| Need ID | Stakeholder | Verbatim Statement | Normalised Need | Category | Priority | Status |
| ------- | ----------- | ------------------ | --------------- | -------- | -------- | ------ |
| NEED-STK-001-001 | {role} | "{exact words}" | {normalised form} | Functional | High | Open |

---

## Conflicts

| Conflict ID | Need A | Need B | Description | Resolution Status |
| ----------- | ------ | ------ | ----------- | ----------------- |
| CONF-001 | | | | Unresolved — carry to /se-conops |

---

## Coverage Check

- Total stakeholders identified: {N}
- Stakeholders with at least one need captured: {N}
- Stakeholders with no needs captured (gap): {list or "None"}
```

### Step 8 — Update Risk Register

Open `specs/system/10-risk/risk-register.md`.
If it does not exist, create it using the template at
`artifacts/risk-register.md` (copy the full header and empty
table structure, then populate the header fields).

**Risk ID assignment:** Before appending new entries, scan
the register for all existing `RISK-NNN` IDs and determine
the maximum N. Assign new IDs starting from `max_N + 1`.
If the register has no entries yet, start at `RISK-001`.
Never derive the next ID from the last row alone — use the
maximum across the entire document to prevent collisions.

For each of the following, add a risk entry if applicable:

- Any stakeholder whose needs were inferred rather than directly
  elicited (unrepresented stakeholder)
- Any need marked as conflicting and unresolved
- Any assumption made about a stakeholder's context that was
  not confirmed during the conversation
- Any stakeholder identified but not interviewed

Use probability P=3 (unknown) and consequence C=4 (significant
impact on stakeholder acceptance) as defaults for elicitation
assumptions unless context clearly suggests otherwise.

Set `Status` = `Open` and `Residual P`, `Residual C`,
`Residual Level` = `—` for all newly created entries.
Update the Summary table counts after adding entries.

### Step 9 — Confirm Completion

Report to the user:

1. Number of stakeholders identified and registered
2. Number of needs captured, by stakeholder
3. Number of conflicts identified
4. Number of risk entries added
5. List of any gaps — stakeholders with no needs, or needs
   marked TBD
6. Recommended next skill: `/thing.se-conops`

Also report the feature delta path, the number of baseline
items added, changed and retired, and every statement corrected
in an artifact this skill does not own.
