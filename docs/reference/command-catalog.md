# Command Catalog

Stable reference for all THING commands.

## Spec-Kit Relationship

THING commands fall into three categories relative to Spec-Kit:

- **New** — no Spec-Kit equivalent; THING introduces the capability
- **Replaces** — supersedes a Spec-Kit command for SE features; the Spec-Kit command is not used in those flows
- **Hook** — runs after a Spec-Kit command, augmenting it without replacing it

Spec-Kit's native skills (`speckit.tasks`, `speckit.analyze`, `speckit.implement`) are used unchanged in phases 4–5. `speckit.specify` and `speckit.plan` are **not used** in the SE feature workflow — they are replaced by `se-requirements` and `se-plan` respectively. They remain available for standard (non-SE) Spec-Kit features, where THING hooks augment them.

## Full Command Reference

| Command | Phase | Spec-Kit Relationship | Purpose |
| ------- | ----- | --------------------- | ------- |
| `se-constitution` | 0 | Hook — runs after `speckit.constitution` | Establish governing document with domain context, standards, development principles, and SE workflow obligations |
| `se-stakeholders` | 0 | New — no Spec-Kit equivalent | Elicit stakeholder needs via structured interview — produces personas, empathy maps, and needs register |
| `se-conops` | 0 | New — no Spec-Kit equivalent | Define Concept of Operations — operational scenarios, modes, and success criteria |
| `se-nfr` | 1 | New — no Spec-Kit equivalent | Elicit non-functional requirements across all ISO 25010 quality categories — seeds V&V plan |
| `se-explore` | 1 | New — no Spec-Kit equivalent | Set-Based Design — define parallel solution options, schedule integration points, record elimination evidence |
| `se-requirements` | 2 | Replaces `speckit.specify` for SE features — SE features have no `spec.md` | Derive functional requirements from ConOps, NFRs, and explore convergence — applies IEEE 29148 inline, writes requirements register, seeds V&V plan and RTM |
| `se-req-quality` | 2 | Runs after `se-requirements`; also hooks after `speckit.specify` for standard Spec-Kit features | Apply IEEE 29148 quality gate to the requirements register — checks all eight quality attributes, produces quality report |
| `se-plan` | 3 | Replaces `speckit.plan` for SE features — SE features have no `spec.md` | Generate SE feature plan from ConOps, NFRs, explore, and requirements — writes `07-plan/plan.md` for `se-architecture` to consume |
| `se-architecture` | 3 | Runs after `se-plan`; also hooks after `speckit.plan` for standard Spec-Kit features | Generate multi-view architecture documentation — C4 diagrams, functional flow, deployment, ADRs |
| `se-interfaces` | 3 | New — no Spec-Kit equivalent | Define Interface Control Document — all interfaces at every system boundary |
| `se-tasks` | 4 | Replaces `speckit.tasks` for SE features — SE features have no `spec.md` | Generate implementation task list from requirements register and plan — organises tasks by requirement (FR-NNN), writes `08-tasks/tasks.md`, updates RTM Task Ref column |
| `se-analyze` | 4 | Replaces `speckit.analyze` for SE features — SE features have no `spec.md` | Cross-artifact consistency analysis — checks requirements register, plan, tasks, and RTM for gaps, ambiguities, terminology drift, and constitution violations |
| `se-vvplan` | 6 | New — no Spec-Kit equivalent | Finalise V&V plan — audit coverage, add validation scenarios, define execution sequence |
| `se-rtm` | 4–6 | Hook (phases 4–5) after `speckit.tasks` and `speckit.implement`; finalised explicitly in phase 6 | Maintain and finalise Requirements Traceability Matrix — audit all four traceability chains, produce traceability report |
| `se-risk` | 6 | New — no Spec-Kit equivalent | Finalise risk register — disposition all risks, produce risk closure report |
| `se-gate` | Each | Hook — runs after `speckit.analyze`; also run explicitly at each phase boundary | Phase gate review — binary pass/fail check before proceeding to next phase |

## Phase Mapping

| Phase | Name | Commands |
|---|---|---|
| 0 | Problem Space | `se-constitution`, `se-stakeholders`, `se-conops` |
| 1 | Solution Space | `se-nfr`, `se-explore` |
| 2 | Requirements | `se-requirements`, `se-req-quality` |
| 3 | Architecture | `se-plan`, `se-architecture`, `se-interfaces` |
| 4 | Implementation Planning | `se-tasks`, `se-analyze` |
| 5 | Implementation | *(Spec-Kit native)* |
| 6 | Verification and Validation | `se-vvplan`, `se-rtm`, `se-risk` |
| Each boundary | Phase Gate | `se-gate` |

## Related Documents

- [Workflow Overview](../guide/workflow-overview.md)
- [THING Architecture](thing-architecture.md)
- [Artifact Schema](artifact-schema.md)
