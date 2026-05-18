# THING — Agent Maintenance Guide

You are maintaining THING: an SE workflow library for AI coding agents.

## What this repository is

THING provides slash commands and artifact templates that guide an AI coding
agent through a full systems engineering lifecycle — from stakeholder
elicitation to requirements, architecture, implementation planning, and
V&V closure.

It is installed as a git submodule in a host project. It is not a standalone
application. It has no runtime, no build step, and no tests in the traditional
sense. Its "tests" are the gate reports produced when a full workflow is run
against a project.

## Repository structure

```text
commands/               Slash command definitions — one .md file per command.
                        Each file is a complete, self-contained instruction
                        set for the AI agent executing that command.
artifacts/              Template files for SE artifacts. Referenced by commands.
docs/
  workflow.md           Workflow sequence reference.
  thing-architecture.md Architecture, operating modes, and workflow paths.
                        Primary reference for understanding THING's design.
extension.yml           Spec-Kit integration shim. Optional. See file header.
AGENT.md                This file.
README.md               User-facing introduction and quick start.
```

## Primary design constraints

1. **Commands are self-contained.** Each command file must specify its own
   pre-conditions, inputs, outputs, and update obligations. An agent executing
   a command reads only that file plus the artifacts it lists as pre-conditions.
   It must not need to read other command files to understand what to do.

2. **Threaded artifact updates are obligations.** The risk register, V&V plan,
   and RTM are updated by almost every command. These updates are never optional.
   Never remove or make conditional the threaded artifact update steps in any
   command.

3. **Pre-condition blocks are contracts.** If a command reads an artifact
   produced by another command, that artifact is listed in the pre-condition
   block. If you add a new artifact to a command's output, add it to the
   pre-condition block of every command that should read it.

4. **The supersede relationship is total for SE features.** THING commands
   completely replace the corresponding spec-kit commands for SE features.
   SE features have no spec.md. Commands must never fall back to reading
   spec.md if SE artifacts are missing. If a pre-condition artifact is
   missing, stop and name the command that produces it.

5. **Slash command namespace is /thing.{name}.** Do not reintroduce any
   previous namespace (speckit.thing, thing) anywhere.

6. **extension.yml is optional infrastructure.** THING's correctness does
   not depend on extension.yml being present or hooks firing. The manual
   invocation sequence documented in docs/thing-architecture.md is the
   primary path. Hooks are a convenience layer on top.

7. **Hook optional flags have fixed semantics:**
   - `optional: false` — hook updates a threaded artifact; skipping breaks
     traceability; never change to true
   - `optional: true` — hook produces a new sequenced artifact; agent may
     legitimately skip on simple features

## When making changes

**Bug fix in a command:** Edit the command file in `commands/`. If the bug
involves a pre-condition check, verify the upstream command that produces
the artifact actually writes it as specified.

**New command:** Add the `.md` file to `commands/`. Add the entry to
`extension.yml` under `provides.commands`. Add the entry to the command
reference table in `README.md`. Add the entry to the command reference
table and inter-skill contract table in `docs/thing-architecture.md`.

**New artifact template:** Add the `.md` file to `artifacts/`. Reference it
from the relevant command file's pre-conditions and output sections.

**Workflow change:** Update `docs/workflow.md` and the relevant path diagram
in `docs/thing-architecture.md`. Update `README.md` if the quick-start
sequence changes.

## What never to do

- Never create a `spec.md` for an SE feature to make a command work.
- Never change a threaded-artifact hook from `optional: false` to `optional: true`.
- Never change a sequenced-artifact hook from `optional: true` to `optional: false`.
- Never write system-level artifacts to `specs/features/` paths or vice versa.
- Never delete rows from the RTM, risk register, or V&V plan. Mark closed,
  accepted, or superseded.
- Never introduce a runtime dependency (package manager, build tool, external
  service). THING is plain Markdown files.
- Never use the ÞING spelling outside of visual branding contexts. It causes
  encoding problems in file names, YAML, and command namespaces.
