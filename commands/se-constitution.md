---
description: "Generate an SE-aligned project constitution that governs all subsequent systems engineering work. Captures domain context, applicable standards, quality principles, and SE workflow obligations. Initiates the risk register with assumption-based seed entries."
---

# SE Constitution

## Purpose

Establish the governing document for all systems engineering work on
this project. This constitution is read by every subsequent SE skill
before taking any action. It defines the problem domain, applicable
standards, quality expectations, and the obligations that apply
throughout the lifecycle.

This skill both creates `.specify/memory/constitution.md` (or updates
it if it already exists) and seeds `specs/system/10-risk/risk-register.md`
with initial assumption-based risk entries.

## Pre-Conditions

None. This is the first skill invoked in the SE workflow.
If a base Spec-Kit constitution already exists, read it first and
preserve any existing content, appending the SE sections below.

## Instructions

### Step 1 — Gather Context

If `$ARGUMENTS` is empty, ask the user the following questions one
section at a time. Do not present all questions at once. Wait for
answers before proceeding to the next section.

### Section A — Project Identity

1. What is the name of this project or system?
2. In one or two sentences, what problem does this system solve?
3. Who are the primary users or operators of this system?
4. What is the operational environment? (e.g. clinical, industrial,
   consumer, embedded, cloud-hosted, field-deployed)

### Section B — Domain and Regulatory Context

1. What industry or domain does this system operate in?
   (e.g. medical device, aerospace, automotive, defence, industrial
   automation, consumer electronics, enterprise software)
2. Are there applicable regulatory frameworks or standards?
   (e.g. ISO 13485, IEC 62304, ISO 14971, DO-178C, ISO 26262,
   MIL-STD-882, IEC 61508, FDA 21 CFR Part 11, CE marking, none)
3. Is this system safety-critical, safety-related, or neither?
4. Is this system subject to configuration control or formal
   change management requirements?

### Section C — Quality and Engineering Principles

1. What are the top three quality attributes most important for
   this system? (e.g. safety, reliability, security, performance,
   usability, maintainability, portability, interoperability)
2. Are there known constraints the solution must respect?
   (e.g. existing hardware, technology stack, budget, timeline,
   team size, open source only)
3. Are there architectural or design principles the team follows?
   (e.g. SOLID, domain-driven design, microservices, MISRA C,
   defence in depth)

### Section C2 — Development Standards

1. What programming language(s) will be used, and is there a
   specific version or standard that must be followed?
   (e.g. Python 3.11+, C++17, C99, Java 17 LTS, TypeScript strict)
   For safety-critical systems: has the language and compiler
   been qualified or does it need to be?
2. What coding style guide or linting standard applies?
   (e.g. PEP 8, Google Style Guide, MISRA C 2023, AUTOSAR C++14,
   ESLint recommended, team-defined rules)
   Where are the linting rules defined? (configuration file in repo,
   external reference, or TBD)
3. What is the minimum required automated test coverage threshold?
   (e.g. 80% line coverage, 100% branch coverage for safety-critical
   functions, no threshold defined)
   What testing framework will be used?
   (e.g. pytest, JUnit, Google Test, Catch2, Jest, Cypress)
4. What CI/CD toolchain will be used?
   (e.g. GitHub Actions, GitLab CI, Jenkins, Azure DevOps, none)
   What pipeline stages are mandatory before merge?
   (e.g. lint, unit test, integration test, security scan,
   coverage gate, build verification)
5. What is the branching strategy?
   (e.g. GitFlow, trunk-based development, feature branches
   with pull request review)
   Are there branch protection rules or required reviewers?
6. Are there any tools that are mandated or prohibited?
   (e.g. mandatory use of a specific SAST tool, prohibition on
   GPL-licensed dependencies, required use of a qualified
   toolchain for safety-critical components)
7. Are there documentation standards beyond this SE pack?
    (e.g. all public APIs must have docstrings, architecture
    diagrams must be reviewed by a lead engineer before merge,
    changelog must be maintained)

### Section D — Stakeholder and Lifecycle Context

1. Who are the key stakeholders beyond the primary users?
    (e.g. regulatory bodies, procurement, maintenance teams,
    operations, executive sponsors)
2. What is the intended system lifecycle?
    (e.g. prototype, product v1, long-term maintained product,
    one-time deployment)
3. Are there existing systems this must interface with?
    List them briefly.

### Step 2 — Write the Constitution

Create or update `.specify/memory/constitution.md` with the
following structure. Use the answers from Step 1 to populate
each section. Where the user has not provided information,
write `TBD — resolve before Phase 1 gate` rather than inventing
content.

```markdown
# Project Constitution

## System Identity

**System Name:** {name}
**Version:** 0.1
**Last Updated:** {date}
**Status:** DRAFT

### Problem Statement
{one paragraph describing the problem this system solves, in the
language of the operational domain, not the solution domain}

### System of Interest
{one paragraph describing what the system is and is not —
explicit boundary statement}

### Primary Users and Operational Environment
{who operates/uses the system and in what context}

---

## Domain and Regulatory Context

**Industry Domain:** {domain}
**Safety Classification:** {safety-critical | safety-related | non-safety}
**Configuration Control Required:** {yes | no}

### Applicable Standards and Frameworks
{bulleted list of standards. For each: standard name, version if
known, and which aspect of the project it governs}

### Regulatory Obligations
{any certification, approval, or submission requirements.
If none: explicitly state "No regulatory obligations identified
at this time."}

---

## Quality Principles

### Priority Quality Attributes
{ranked list of the top quality attributes with a one-sentence
explanation of why each matters for this system}

### Known Constraints
{bulleted list. For each constraint: what it is, why it exists,
and which phases it affects}

### Engineering and Design Principles
{bulleted list of principles the team follows.
If none stated: default to the following and note they are defaults:
- Requirements shall be verifiable before being baselined
- Every design decision shall have documented rationale (ADR)
- No requirement shall be implemented without a corresponding
  V&V plan entry
- Architecture shall be documented in multiple views (C4 model)
- All SE artifacts shall be version-controlled as code}

### Development Standards

**Language and Compiler:**
{language, version, standard, qualification status if safety-critical}

**Coding Style and Linting:**
{style guide reference, linting tool and config file location}

**Test Coverage Threshold:**
{minimum coverage percentage and scope, testing framework}

**CI/CD Pipeline:**
{toolchain, mandatory pipeline stages, gate criteria for merge}

**Branching Strategy:**
{strategy name, branch protection rules, required reviewers}

**Mandated or Prohibited Tools:**
{list or "None identified at this time"}

**Documentation Standards:**
{any documentation obligations beyond the SE pack}

---

## Stakeholder Context

**Key Stakeholders:** {list with role and interest}
**System Lifecycle:** {prototype | v1 product | long-term maintained | other}

### External System Interfaces (Known)
{list of known external systems. If none known yet: "None identified
at this time — to be populated in /se-interfaces"}

---

## SE Workflow Obligations

This project follows the THING workflow. The following
obligations apply to every skill invocation:

### Threaded Artifact Update Obligations

| Artifact | Initiated | Every skill must |
| -------- | --------- | ---------------- |
| `risk-register.md` | Phase 0 | Add entries for any new assumptions, unknowns, or external dependencies introduced |
| `vv-plan.md` | Phase 1 | Add an entry for every new NFR or requirement |
| `rtm.md` | Phase 2 | Update allocations when design, tasks, or implementation changes |

### Phase Gate Obligations

No phase is complete until `/thing.se-gate {N}` passes.
Gates check threaded artifact currency, not just sequenced artifact
existence.

### Requirements Quality Obligations

Every requirement must satisfy all eight IEEE 29148 quality attributes
before Phase 2 closes:
Necessary · Unambiguous · Verifiable · Consistent · Complete ·
Singular · Feasible · Traceable

A requirement without a verification method is **incomplete by
definition**. The V&V plan entry must exist before the requirement
is baselined.

### Architecture Documentation Obligations

All architecture views shall be produced as Mermaid diagrams embedded
in Markdown files. The minimum required views are:
- C4 Level 1: System Context
- C4 Level 2: Container
- Functional Flow
- Deployment/Allocation

Architecture Decision Records (ADRs) shall be written for every
significant design decision using the format:
Context → Decision → Consequences → Alternatives Considered

### Set-Based Design Obligation

Until Phase 3 gate passes, no solution option shall be eliminated
without documented evidence. Opinion is not evidence.
All eliminated options shall have an entry in `elimination-record.md`
stating what evidence caused elimination.

---

## Artifact Structure

**System Artifacts Root:** `specs/system/`
**Feature Artifacts Root:** `specs/features/`

System-level SE artifacts live at `specs/system/` and are updated
in place with each feature iteration. Feature-scoped artifacts are
created fresh per feature at `specs/features/{feature-id}/`.

| Artifact | Canonical Path |
| -------- | -------------- |
| Stakeholder register, personas, needs register | `specs/system/00-stakeholders/` |
| ConOps, operational scenarios | `specs/system/01-conops/` |
| Quality attributes (NFRs) | `specs/system/02-nfr/` |
| Requirements register, quality report | `specs/system/04-requirements/` |
| Architecture documentation, views, ADRs | `specs/system/05-architecture/` |
| Interface Control Document | `specs/system/06-interfaces/` |
| V&V plan and results | `specs/system/09-vv/` |
| Risk register | `specs/system/10-risk/` |
| Requirements Traceability Matrix | `specs/system/rtm.md` |
| Set-Based Design exploration | `specs/features/{feature-id}/03-explore/` |
| Feature plan | `specs/features/{feature-id}/07-plan/` |
| Implementation tasks | `specs/features/{feature-id}/08-tasks/` |
| Phase gate reports | `specs/features/{feature-id}/gate-reports/` |

---

## Open Issues and TBDs

{table of any TBD items from above, with owner and target resolution
phase}

| ID | Item | Owner | Target Phase |
| -- | ---- | ----- | ------------ |
```

### Step 3 — Seed the Risk Register

Create `specs/system/10-risk/risk-register.md` if it does not exist.

For each assumption made during Step 1 — things the user stated
as fact but which have not been verified — create a risk entry.
Also create entries for any TBDs recorded in the constitution.

Use this format for each entry:

```markdown
| {ID} | {description} | Assumption/Unknown | {P} | {C} | {P×C} | Monitor | {what would confirm or deny this assumption} | Phase 0 |
```

Risk probability (P) and consequence (C) are scored 1–5.
Risk level = P × C. Initial entries from assumptions should
default to P=3 (unknown) unless context suggests otherwise.

Columns: ID · Description · Category · Probability(1-5) ·
Consequence(1-5) · Risk Level · Handling Strategy ·
Mitigation/Evidence Action · Phase Identified

### Step 4 — Confirm Completion

Report to the user:

1. Path and status of constitution.md (created or updated)
2. Number of risk register seed entries created
3. List of any TBD items that must be resolved before Phase 1
4. Recommended next skill: `/thing.se-stakeholders`
