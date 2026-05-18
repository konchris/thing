# THING Color Tokens

THING should not use a “marketing palette.”

The colors should behave like infrastructure: stable, legible, restrained, and repeatable across GitHub, documentation, diagrams, terminal screenshots, and the later website.

The palette is built around:

- **Ink**: seriousness, primary identity, high contrast
- **Paper / Parchment**: documentation, accumulated knowledge, handbook quality
- **Stone / Line**: diagrams, artifact structure, traceability
- **Indigo / Violet**: restrained intellectual accent, not AI hype
- **Moss / Risk**: optional semantic accents, used sparingly

---

## Primary Tokens

| Token | Hex | Role |
|---|---:|---|
| `--thing-bg` | `#F3F1EC` | Primary documentation background / parchment off-white |
| `--thing-surface` | `#FAF8F3` | Cards, README panels, light surfaces |
| `--thing-ink` | `#161616` | Primary text, logo, high-contrast linework |
| `--thing-iron` | `#252A2E` | Dark UI backgrounds, terminal panels |
| `--thing-stone` | `#6E6B67` | Secondary text, muted labels |
| `--thing-line` | `#C9C5BD` | Rules, dividers, diagram grid lines |
| `--thing-line-strong` | `#8B8780` | Diagram emphasis lines and outlines |
| `--thing-indigo` | `#273248` | Primary accent; serious, technical, calm |
| `--thing-violet` | `#4A4458` | Secondary accent; scholarly, restrained |
| `--thing-moss` | `#68736D` | Optional semantic accent for stable/approved states |
| `--thing-risk` | `#8A5A44` | Optional restrained warning/risk accent |
| `--thing-white` | `#FFFFFF` | Pure white only when platform constraints require it |

---

## Recommended Usage

### Documentation

Use `--thing-bg` or `--thing-surface` as the background, with `--thing-ink` for text.

Use `--thing-stone` for secondary explanatory text.

Use `--thing-line` for separators, table borders, diagram grids, and subtle structure.

### README

GitHub does not allow arbitrary CSS in Markdown, so use the palette mainly in:

- SVG header images
- diagrams
- social preview image
- screenshots
- badges where possible
- generated documentation assets

### Diagrams

Default diagram style:

- background: `--thing-bg`
- primary linework: `--thing-ink`
- secondary linework: `--thing-stone`
- grid or trace lines: `--thing-line`
- selected/emphasized path: `--thing-indigo`

Avoid using many colors in one diagram. A good THING diagram usually needs only ink, line, stone, and one accent.

### Terminal Screenshots

Use:

- background: `--thing-iron`
- foreground: `--thing-bg`
- muted text: `--thing-line`
- accent prompt or command: `--thing-indigo` or `--thing-violet`

### Semantic Use

Use semantic color sparingly.

| Semantic Meaning | Token |
|---|---|
| Approved / stable / verified | `--thing-moss` |
| Risk / concern / unresolved | `--thing-risk` |
| Primary emphasis | `--thing-indigo` |
| Secondary emphasis | `--thing-violet` |

Do not turn THING into a traffic-light dashboard unless the artifact specifically requires status communication.

---

## Anti-Rules

Avoid:

- neon AI blues
- glowing cyan/purple gradients
- excessive semantic colors
- dark mode with cyberpunk contrast
- “startup purple” as the dominant identity
- large saturated blocks of color
- decorative gradients
- color-only meaning without labels

---

## Palette Character

The intended impression:

> quiet, technical, durable, legible, historically aware

The color system should feel like it belongs to:

- a systems engineering handbook
- a scientific notebook
- a Unix-fluent documentation site
- an architecture diagram

not:

- an AI SaaS landing page
- a gaming project
- a cyberpunk CLI toy

---

## Included Files

- `thing-colors.css`
- `thing-colors.tokens.json`
- `thing-color-preview.html`
- `thing-color-preview.png`
