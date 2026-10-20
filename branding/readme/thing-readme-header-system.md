# THING README Header System

This package defines the README identity layer for THING.

The README should not feel like a landing page. It should feel like the opening page of a well-maintained engineering handbook.

---

## Included Files

| File | Purpose |
| --- | --- |
| `thing-readme-header.svg` | Primary README header |
| `thing-readme-header-compact.svg` | Smaller README header for dense pages |
| `thing-readme-header-dark.svg` | Dark / terminal-oriented variant |
| `thing-readme-symbol.svg` | Symbol-only SVG for README sections or small placements |
| `README-header-example.md` | Example Markdown usage |

---

## Recommended Repository Placement

```text
branding/
  readme/
    thing-readme-header.svg
    thing-readme-header-compact.svg
    thing-readme-header-dark.svg
    thing-readme-symbol.svg
```

Then reference the primary header from the repository `README.md`:

```markdown
<p align="center">
  <img src="branding/readme/thing-readme-header.svg" alt="THING — Systems Engineering Workflow Library for AI Coding Agents" width="960">
</p>
```

---

## Preferred README Opening

```markdown
# THING

**Systems engineering workflows for AI coding agents.**

THING is a Markdown-based workflow library for guiding AI coding agents through systems engineering activities before implementation begins.
```

This should come immediately after the header image.

---

## Header Usage Rules

### Use the primary header when

- presenting the main repository
- creating the first visual impression
- linking the project from documentation
- creating a clean top-of-page identity

### Use the compact header when

- the page already has many diagrams
- the header should stay visually quiet
- the page is a subdocument rather than the project root

### Use the dark header when

- the page emphasizes CLI/terminal usage
- the surrounding visual context is dark
- the document is about tooling, commands, or execution

---

## Visual Rules

The README should use:

- sparse layout
- short declarative headings
- calm technical language
- few badges
- restrained diagrams
- table structure where useful

Avoid:

- banner clutter
- animated GIFs above the fold
- long badge rows
- marketing slogans
- hype language
- generic AI imagery
- “startup landing page” formatting

---

## Badge Guidance

Use badges only if they carry real engineering value.

Good candidates:

```markdown
![License](https://img.shields.io/github/license/OWNER/REPO)
![CI](https://img.shields.io/github/actions/workflow/status/OWNER/REPO/ci.yml)
![Docs](https://img.shields.io/badge/docs-living_specification-273248)
```

Avoid decorative badges that merely add noise.

---

## Section Header Convention

Recommended top-level README sections:

```markdown
## What THING Is

## What THING Is Not

## Why It Exists

## Workflow Library Structure

## Example Use

## Installation / Copying

## Documentation

## Contributing
```

This reinforces the project’s disciplined identity.

---

## Tone

The README should sound like:

- engineering handbook
- Unix tool
- architecture note
- scientific notebook

It should not sound like:

- SaaS copy
- AI prompt-pack marketplace
- enterprise consulting material
- “10x developer” rhetoric

---

## Suggested README Tagline

```text
Structure before implementation.
```

Alternative:

```text
Disciplined systems engineering workflows for AI coding agents.
```

Alternative:

```text
Markdown workflows for governing complexity before code.
```

The first is strongest if you want a compact identity.
The second is clearest.
The third is most conceptually aligned.
