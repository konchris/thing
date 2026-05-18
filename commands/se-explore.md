---
description: "Conduct Set-Based Design exploration of solution options. Defines the solution set, establishes parallel option workstreams, schedules integration points for evidence-based elimination, and records elimination rationale. Prevents premature convergence on a single solution before evidence justifies it."
---

# SE Explore

## Purpose

Apply Set-Based Design (SBD) to explore the solution space before
committing to an architecture. Rather than selecting one solution
early and refining it (Point-Based Design), this skill defines a
*set* of viable options bounded by constraints, runs them in
parallel at appropriate fidelity, and eliminates options only when
*evidence* rules them out.

This skill is grounded in SAFe Principle #3: *Assume variability;
preserve options*, and in the lean product development practice
pioneered at Toyota.

**Opinion is not evidence. An option is not eliminated until
evidence from exploration — analysis, prototype, test, or
research — demonstrates it cannot satisfy the constraints.**

Output artifacts:

- `specs/features/$FEATURE_ID/03-explore/explore-summary.md`
- `specs/features/$FEATURE_ID/03-explore/option-{id}/description.md`
- `specs/features/$FEATURE_ID/03-explore/option-{id}/assumptions.md`
- `specs/features/$FEATURE_ID/03-explore/option-{id}/tasks.md`
- `specs/features/$FEATURE_ID/03-explore/option-{id}/evaluation-log.md`
- `specs/features/$FEATURE_ID/03-explore/elimination-record.md`

Updates:

- `specs/system/10-risk/risk-register.md`

## Pre-Conditions

- `.specify/memory/constitution.md` must exist. Read it.
- `specs/system/00-stakeholders/needs-register.md`
  must exist. Read it.
- `specs/system/02-nfr/quality-attributes.md` must exist.
  Read it. NFRs and their priorities are the primary evaluation
  criteria for option elimination.
- `specs/system/01-conops/conops.md` must exist. Read it.
  Operational scenarios define the conditions options must satisfy.

If any pre-condition is not met, stop and tell the user which
skill must be run first.

## Instructions

### Step 1 — Read Context

Read all pre-condition artifacts. Build an internal picture of:

- The core problem being solved
- The Must Have NFRs — these are hard constraints that any
  surviving option must satisfy
- The Should Have NFRs — these differentiate between options
- The operational scenarios — particularly failure and degraded
  operation scenarios, which stress options hardest
- Any constraints in the constitution (technology, budget,
  team capability, existing systems)
- Any known external system interfaces that constrain the
  solution space

Summarise the constraints back to the user before proceeding,
and ask them to confirm or correct your understanding.

### Step 2 — Define the Solution Set Boundaries

Tell the user: *"Before we identify specific options, we need
to define the boundaries of the solution space — what any valid
solution must satisfy, and what would immediately disqualify
an option. These boundaries define the set."*

Ask:

1. Are there any hard constraints that immediately eliminate
   entire classes of solution?
   (e.g. must use existing hardware, must be open source,
   must not require cloud connectivity, must be certifiable
   under IEC 62304)
2. Are there any solutions already known to have been tried
   and failed? What was the evidence of failure?
3. Are there any solutions the team strongly favours?
   (Note: this is valid input, but preference is not evidence —
   it will be recorded as a hypothesis to be tested, not a
   decision)
4. What is the maximum number of parallel options the team
   can realistically explore given time and resources?
   (Typical range: 2–4. More than 4 is usually unmanageable.)
5. By what date or milestone must the solution set converge
   to a single option? This is the final integration point.

### Step 3 — Generate Solution Options

Tell the user: *"Now let's identify the candidate solutions.
We want genuine alternatives — options that approach the problem
differently, not minor variations of the same approach. The goal
is to preserve real optionality, not create artificial diversity."*

For each candidate option the user proposes or that you can
infer from the domain context:

1. Ask the user to describe the option in one paragraph —
   what is the core approach?
2. Ask: what are the key assumptions this option depends on
   being true?
3. Ask: which Must Have NFRs is this option most at risk of
   failing to satisfy, and why?
4. Ask: what is the fastest way to find out if this option
   is viable?

If the user proposes only one option, challenge this directly:
*"You've described one approach. Before we commit to it, let's
identify at least one genuinely different alternative — even
if you believe the first option is correct. Set-Based Design
requires real alternatives to be meaningful."*

Assign each option an ID: `OPT-{A|B|C|...}`

### Step 4 — Define Integration Points

Tell the user: *"Integration points are scheduled moments where
we review the evidence gathered for each option and decide
whether to eliminate any. They are not progress reviews —
they are elimination decisions. An option that survives an
integration point continues. An option eliminated at an
integration point is permanently closed with documented
rationale."*

Ask:

1. How many integration points make sense before the final
   convergence milestone?
   (Typical: 2–3 intermediate points plus the final)
2. For each integration point, what question must be answered?
   (e.g. "By IP-1: can option B satisfy NFR-SEC-001 without
   requiring a hardware security module?")
3. What evidence would be sufficient to eliminate an option
   at each integration point?
   (e.g. prototype benchmark result, expert analysis,
   vendor confirmation, regulatory pre-submission feedback)

Assign each integration point an ID: `IP-{NNN}`
Record the date or milestone, the question to be answered,
and the evidence threshold for elimination.

### Step 5 — Define Exploration Tasks Per Option

For each surviving option, define the tasks needed to generate
the evidence required at the first integration point. These
are not implementation tasks — they are *learning tasks*:
the minimum work needed to answer the key question for this
option.

For each option ask:

1. What is the single most important unknown about this option?
2. What is the smallest experiment, prototype, or analysis
   that would answer that unknown?
3. Who would do that work and roughly how long would it take?
4. What does a positive result look like? What does a negative
   result look like?

### Step 6 — Write Output Artifacts

#### 6a — Explore Summary

Write `specs/features/$FEATURE_ID/03-explore/explore-summary.md`:

```markdown
# Solution Exploration Summary

**System:** {system name}
**Feature:** {feature ID and name}
**Date:** {date}
**Status:** ACTIVE — options under exploration
**Last Updated By:** se-explore skill

---

## Solution Set Boundaries

### Hard Constraints (Immediate Disqualifiers)

{bulleted list — any option violating these is immediately
eliminated}

### Soft Constraints (Differentiators)

{bulleted list — NFRs and preferences that differentiate
between options but do not immediately eliminate}

### Known Failed Approaches

| Approach | Evidence of Failure | Source |
| -------- | ------------------- | ------ |

---

## Active Options

| Option ID | Name | Core Approach | Status | Eliminated At |
| --------- | ---- | ------------- | ------ | ------------- |
| OPT-A | {name} | {one sentence} | Active | — |

---

## Integration Points

| IP ID | Milestone/Date | Question to Answer | Evidence Threshold | Status |
| ----- | -------------- | ------------------ | ------------------ | ------ |
| IP-001 | {date} | {question} | {what counts as evidence} | Pending |

---

## Convergence Target

**Final Integration Point:** {date or milestone}
**Convergence Criterion:** {what must be true for the set
to converge to a single option}

---

## Current Status

**Options Active:** {N}
**Options Eliminated:** {N}
**Next Integration Point:** {IP-ID} on {date}
```

#### 6b — Option Description

For each option, write
`specs/features/$FEATURE_ID/03-explore/option-{id}/description.md`:

```markdown
# Option {ID}: {Name}

**Status:** Active | Eliminated
**Date Created:** {date}
**Last Updated:** {date}

---

## Core Approach

{One to two paragraphs describing the approach in plain language.
No implementation detail — conceptual level only.}

## Key Characteristics

{Bulleted list of the defining properties of this option —
what makes it distinct from the alternatives}

## NFR Satisfaction Assessment

| NFR ID | Priority | Assessment | Confidence |
| ------ | -------- | ---------- | ---------- |
| NFR-PERF-001 | Must Have | Likely satisfied | Low — assumption |

## Potential Advantages

{What this option does better than alternatives}

## Potential Weaknesses

{Where this option is most at risk}
```

#### 6c — Option Assumptions

For each option, write
`specs/features/$FEATURE_ID/03-explore/option-{id}/assumptions.md`:

```markdown
# Assumptions: Option {ID}

**Date:** {date}

---

## Assumptions Register

| ID | Assumption | If False, Then | Validation Approach | Status |
| -- | ---------- | -------------- | ------------------- | ------ |
| ASM-{OPT}-001 | {what we believe to be true} | {consequence of being wrong} | {how we will confirm} | Unvalidated |

---

## Assumption Risk Summary

{Which assumptions pose the highest risk if wrong,
and at which integration point they must be resolved}
```

#### 6d — Option Tasks

For each option, write
`specs/features/$FEATURE_ID/03-explore/option-{id}/tasks.md`:

```markdown
# Exploration Tasks: Option {ID}

**Target Integration Point:** {IP-ID}
**Date:** {date}

---

## Tasks

| Task ID | Description | Type | Owner | Effort | Target IP | Output |
| ------- | ----------- | ---- | ----- | ------ | --------- | ------ |
| TASK-{OPT}-001 | {what to do} | Prototype \| Analysis \| Research \| Test | TBD | {estimate} | {IP-ID} | {deliverable} |

---

## Evidence Collection

{What will be recorded at the end of each task to feed
the integration point evaluation}
```

#### 6e — Option Evaluation Log

For each option, create
`specs/features/$FEATURE_ID/03-explore/option-{id}/evaluation-log.md`:

```markdown
# Evaluation Log: Option {ID}

---

## Integration Point Reviews

### {IP-ID} — {date}

**Question:** {the question this IP was meant to answer}
**Evidence Gathered:** {what was learned}
**Assessment:** {does the evidence support continuation or elimination?}
**Decision:** Continue | Eliminate
**Decision Rationale:** {why — reference evidence specifically}
**Decision Made By:** {who}

---
```

#### 6f — Elimination Record

Create `specs/features/$FEATURE_ID/03-explore/elimination-record.md`:

```markdown
# Option Elimination Record

**System:** {system name}
**Feature:** {feature ID and name}

---

*This document records all options that were eliminated during
exploration, and the evidence that justified elimination.
Eliminated options are never deleted — they are preserved here
so that future engineers understand why the road not taken
was abandoned.*

---

## Eliminated Options

| Option ID | Name | Eliminated At | Evidence Summary | Decision By |
| --------- | ---- | ------------- | ---------------- | ----------- |

---

## Detail Records

{One section per eliminated option, added when elimination occurs}
```

### Step 7 — Update Risk Register

Add risk entries for:

- Each option's highest-risk assumption (from 6c)
- Any integration point where the evidence threshold is
  unclear or difficult to obtain
- Any option where team capability to execute exploration
  tasks is uncertain

### Step 8 — Confirm Completion

Report to the user:

1. Number of options defined and currently active
2. Number of integration points scheduled
3. Total exploration tasks defined across all options
4. Number of assumptions registered
5. Number of risk entries added
6. Remind the user: options are not eliminated here —
   they are eliminated at integration points based on evidence.
   This skill sets up the framework; the team runs the exploration.
7. Recommended next skill: `/thing.se-gate 1`
   (Phase 1 gate before proceeding to requirements)
