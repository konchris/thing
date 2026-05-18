# THING Typography Definition

## Purpose

Typography for THING should make the project feel like a disciplined engineering library: precise, calm, readable, and durable.

The typography should support:

- Markdown-first documentation
- GitHub README readability
- terminal and code-adjacent presentation
- diagrams and systems engineering artifacts
- eventual website use
- long-term maintainability

It should avoid:

- sci-fi fonts
- fake rune fonts
- startup branding typography
- overly playful display fonts
- anonymous enterprise blandness
- typographic decoration that reduces legibility

---

## Recommended Primary Typography System

### Primary Recommendation

| Role | Font | Rationale |
|---|---|---|
| Logo / Wordmark | **Space Grotesk** | Geometric, technical, slightly unusual, but still restrained. Works well for a project named THING without becoming gimmicky. |
| Headings | **IBM Plex Sans** | Engineered, readable, institutional without being corporate. Good documentation tone. |
| Body | **Inter** | Excellent screen readability, widely used, neutral, reliable. |
| Code / Monospace | **IBM Plex Mono** | Technical but humane. Pairs naturally with IBM Plex Sans. |

This system gives THING a grounded technical identity while avoiding both AI-startup slickness and fake historical styling.

---

## Alternative Typography System

### More Scholarly / Handbook-Oriented

| Role | Font | Rationale |
|---|---|---|
| Logo / Wordmark | **Space Grotesk** | Keeps the identity modern and technical. |
| Display / Major Headings | **IBM Plex Serif** | Adds a handbook / scholarly tone without becoming academic-heavy. |
| UI / Navigation | **IBM Plex Sans** | Keeps interfaces clear and technical. |
| Body | **Inter** | Maintains practical readability. |
| Code | **JetBrains Mono** | Excellent for technical documentation and code examples. |

Use this version if the website should feel more like a serious engineering handbook than a software tool landing page.

---

## Minimal System Font Stack

If external fonts should be avoided, use a system-first stack.

```css
--font-sans: ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
--font-serif: ui-serif, Georgia, Cambria, "Times New Roman", Times, serif;
--font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
```

This is less distinctive, but very robust.

---

## Recommended CSS Tokens

```css
:root {
  --thing-font-logo: "Space Grotesk", ui-sans-serif, system-ui, sans-serif;
  --thing-font-heading: "IBM Plex Sans", ui-sans-serif, system-ui, sans-serif;
  --thing-font-body: "Inter", ui-sans-serif, system-ui, sans-serif;
  --thing-font-code: "IBM Plex Mono", ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
}
```

For a more handbook-oriented site:

```css
:root {
  --thing-font-logo: "Space Grotesk", ui-sans-serif, system-ui, sans-serif;
  --thing-font-display: "IBM Plex Serif", ui-serif, Georgia, serif;
  --thing-font-heading: "IBM Plex Sans", ui-sans-serif, system-ui, sans-serif;
  --thing-font-body: "Inter", ui-sans-serif, system-ui, sans-serif;
  --thing-font-code: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
}
```

---

## Type Hierarchy

### Documentation / Website

| Element | Font | Weight | Notes |
|---|---:|---:|---|
| Project title | Space Grotesk | 600–700 | Use sparingly. Avoid excessive letter spacing. |
| H1 | IBM Plex Sans | 600 | Clear and calm. |
| H2 | IBM Plex Sans | 600 | Section-level structure. |
| H3 | IBM Plex Sans | 500–600 | Subsection structure. |
| Body | Inter | 400 | Default reading text. |
| Emphasis | Inter | 500 | Prefer weight over color. |
| Code | IBM Plex Mono | 400 | Use for commands, paths, IDs, artifact names. |
| Captions | Inter | 400 | Slightly smaller, muted color. |
| Diagram labels | IBM Plex Sans or IBM Plex Mono | 400–500 | Sans for conceptual diagrams, mono for artifact/traceability labels. |

---

## Logo Typography Guidance

The wordmark should be simple and robust.

Recommended treatment:

```text
THING
```

Characteristics:

- uppercase
- no fake runic styling
- no decorative cuts
- no distressed texture
- modest tracking only if needed
- symbol and wordmark should align cleanly on a horizontal axis

Optional branding-only variant:

```text
ÞING
```

Use `ÞING` sparingly. It is conceptually strong, but too much use may make the identity feel like a novelty Norse brand rather than a serious engineering tool.

Recommended rule:

- Use **THING** as the normal project name.
- Use **Þ** as a symbol or hidden-detail element.
- Use **ÞING** only in special branding contexts, not in package names, commands, or technical docs.

---

## README Typography Guidance

GitHub Markdown limits font control, so identity should come from structure, spacing, and restrained use of symbols.

Recommended README header:

```markdown
# THING

**Systems Engineering Workflow Library for AI Coding Agents**

Structure in service of coherence.
```

Avoid excessive badges at the top. If badges are used, keep them in one restrained row below the project description.

Recommended code/path formatting:

```markdown
`stakeholder-analysis.md`
`conops.md`
`interface-control.md`
`verification-matrix.md`
```

Use tables for systems engineering artifacts where possible.

---

## Diagram Typography Guidance

Diagrams should feel like engineering artifacts, not marketing illustrations.

Use:

- IBM Plex Sans for conceptual nodes
- IBM Plex Mono for artifact names, IDs, requirements, traceability markers
- regular or medium weights
- small but legible labels
- consistent capitalization

Avoid:

- all-caps everywhere
- decorative labels
- emoji markers
- excessive color coding
- pseudo-handwritten diagram fonts

Example node label style:

```text
Stakeholder Need
System Requirement
Architecture Element
Verification Case
Risk Control
```

Example traceability label style:

```text
REQ-014
ARCH-003
TEST-021
RISK-007
```

---

## Terminal / CLI Typography Guidance

Use monospace identity sparingly.

Recommended terminal signature:

```text
[þ] THING
```

or:

```text
:: THING ::
```

Avoid elaborate ASCII art. THING should feel like a serious Unix-adjacent tool, not a banner generator.

---

## Naming and Capitalization Rules

Recommended:

- Project name: **THING**
- Optional symbolic glyph: **þ** or **Þ**
- Repository/package references: `thing`
- Commands or paths: lowercase kebab-case or snake_case, depending on ecosystem

Examples:

```text
THING
thing-workflows
thing/templates/conops.md
thing trace check
```

Avoid:

```text
ThIng
THING.ai
ThingOS
THINGCloud
THING Enterprise
```

---

## Practical Font Sources

All recommended fonts are available from common open font sources:

- Space Grotesk
- IBM Plex Sans
- IBM Plex Serif
- IBM Plex Mono
- Inter
- JetBrains Mono
- Source Sans 3

Do not commit font binaries unless license handling is deliberate and documented. Prefer referencing open fonts through the documentation website build system or using system fallbacks for GitHub-first usage.

---

## Final Recommendation

Use this as the default THING typography system:

```text
Logo:     Space Grotesk
Headings: IBM Plex Sans
Body:     Inter
Code:     IBM Plex Mono
```

This gives THING the right balance:

- technical but humane
- modern but not trendy
- precise but not sterile
- distinctive without becoming decorative
- compatible with GitHub, documentation, diagrams, and a future website
