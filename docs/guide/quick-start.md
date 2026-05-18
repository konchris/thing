# Quick Start

Run the smallest useful THING workflow against a repository.

## Prerequisites

- Git repository
- AI coding agent capable of reading Markdown command files (Claude Code or equivalent)
- THING installed as a submodule or copied into the project

## Install THING

```bash
git submodule add https://github.com/konchris/thing .thing
```

This pins the project to a specific THING version. Updates are explicit:

```bash
git submodule update --remote --merge
```

## Run the First Workflow

Tell the agent:

```text
Read `.thing/commands/se-stakeholders.md` and execute it for this project.
```

Or, if registered as a slash command:

```text
/thing.se-stakeholders
```

## Recommended First Three Commands

Run these in order. Each produces durable artifacts that the next command reads.

1. `se-stakeholders` — establish who the system is for and what they need
2. `se-conops` — define how the system is used in operational context
3. `se-nfr` — elicit quality and constraint requirements

## Expected Outputs

| Command | Produces |
|---|---|
| `se-stakeholders` | stakeholder register, personas, empathy maps, needs register |
| `se-conops` | operational scenarios, modes, success criteria |
| `se-nfr` | quality attribute requirements, V&V plan seeds |

All outputs are plain Markdown files committed to the repository under `specs/system/`.

## Choose Your Path

If the system already has existing SE artifacts, you may not need to start at phase 0.

| Situation | Start here |
|---|---|
| New system, no prior artifacts | `se-stakeholders` (Phase 0) |
| Existing artifacts, new feature with new requirements | `se-requirements` (Phase 2) |
| Requirements defined, bounded implementation | Standard Spec-Kit + THING hooks |

See [Workflow Overview](workflow-overview.md) for the full phase sequence and path selection guide.
