---
title: <Workflow Name>
type: workflow
status: draft
owner: maintainers
last_reviewed: YYYY-MM-DD
traces_to: []
---

# <Workflow Name>

## Purpose

Describe the engineering uncertainty this workflow reduces.

## When to Use

Use this workflow when:

- condition one
- condition two
- condition three

## Inputs

| Input | Description | Required |
| --- | --- | --- |
| `<input-artifact>` | Description | yes/no |

## Outputs

| Output | Description |
| --- | --- |
| `<output-artifact>` | Description |

## Workflow Steps

1. **Frame the problem**  
   Define the boundary of the activity.

2. **Elicit structure**  
   Extract relevant constraints, concerns, decisions, and unknowns.

3. **Record durable artifacts**  
   Write the result in a form that can be traced and reviewed.

4. **Check quality**  
   Evaluate against the criteria below.

## Quality Criteria

The artifact is acceptable when:

- [ ] it has a clear purpose
- [ ] assumptions are explicit
- [ ] downstream decisions can use it
- [ ] verification or validation implications are visible
- [ ] unresolved risks are named

## Traceability Hooks

| Source | Relationship | Target |
| --- | --- | --- |
| `<source-id>` | motivates / constrains / verifies / mitigates | `<target-id>` |

## Common Failure Modes

| Failure Mode | Symptom | Correction |
| --- | --- | --- |
| Prose without structure | The document reads well but cannot be traced | Add IDs, tables, and explicit relationships |
| Premature implementation | The artifact jumps to code or solution details | Return to intent and constraints |
| Generic AI output | The artifact sounds plausible but is not project-specific | Add concrete system context |

## Example

Provide a small worked example here.
