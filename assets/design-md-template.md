---
brand: "{{BRAND_NAME}}"
version: "1.0.0"
generated: "{{DATE}}"
generator: "brand-interview-skill"
colors:
  primary: "{{HEX}}"
  secondary: "{{HEX}}"
  tertiary: "{{HEX}}"
  background: "{{HEX}}"
  surface: "{{HEX}}"
  text: "{{HEX}}"
  text-muted: "{{HEX}}"
  accent: "{{HEX}}"
  error: "{{HEX}}"
  success: "{{HEX}}"
typography:
  heading: "{{FONT_STACK}}"
  body: "{{FONT_STACK}}"
  mono: "{{FONT_STACK}}"
  size-base: "{{PX}}"
  scale: "{{RATIO}}"
rounded:
  sm: "{{PX}}"
  md: "{{PX}}"
  lg: "{{PX}}"
  full: "9999px"
spacing:
  unit: "{{PX}}"
  scale: "{{RATIO_OR_LIST}}"
components: []
---

# {{BRAND_NAME}} — Design System

Derived from the winning visual direction (Phase 4) and the §7 Visual Direction answers in `BRAND.md`. Token values above are the source of truth — every section below should read as a description of those values, never introduce a color, font, or measurement that isn't in the frontmatter.

## Visual Theme & Atmosphere

{{2-3 sentences: the overall visual mood (e.g. "warm minimalism," "confident editorial") and what it should make someone feel in the first second of seeing the brand}}

## Color Palette & Roles

List every role from the `colors:` frontmatter block, one per line, in this exact format:
- **primary**: `primary` — `{{HEX}}`
- **secondary**: `secondary` — `{{HEX}}`
- **tertiary**: `tertiary` — `{{HEX}}`
- **background**: `background` — `{{HEX}}`
- **surface**: `surface` — `{{HEX}}`
- **text**: `text` — `{{HEX}}`
- **text-muted**: `text-muted` — `{{HEX}}`
- **accent**: `accent` — `{{HEX}}`
- **error**: `error` — `{{HEX}}`
- **success**: `success` — `{{HEX}}`

Then 1-2 sentences on when to reach for `accent` vs `primary`, and any contrast/accessibility notes (e.g. minimum text-on-background ratio).

## Typography Rules

- **Heading font**: `{{FONT_STACK}}` — {{why it fits the brand}}
- **Body font**: `{{FONT_STACK}}` — {{why it fits the brand}}
- **Mono font**: `{{FONT_STACK}}` (code/data contexts only)
- **Base size**: `{{PX}}`, **scale**: `{{RATIO}}`

## Component Stylings

{{Corner radius (`rounded.sm/md/lg/full`), border/shadow treatment, button and card conventions. If no components exist yet, say so plainly and describe the intended feel for buttons/cards/inputs when they're built.}}

## Layout Principles

{{Grid/spacing rhythm — reference `spacing.unit` and `spacing.scale` from frontmatter. Density (airy vs compact), alignment conventions.}}

## Depth & Elevation

{{Shadow/layering conventions — flat, subtle, or pronounced. Should match the theme mode (`light`/`dark`) captured in §7.}}

## Do's and Don'ts

- Do: {{2-3 concrete dos tied to the actual tokens above}}
- Don't: {{2-3 concrete don'ts — e.g. "don't use `error` red as a decorative accent"}}

## Responsive Behavior

{{How the type scale, spacing, and layout should adapt from mobile to desktop — 2-3 sentences, not a full breakpoint spec unless the interview covered one.}}

## Agent Prompt Guide

{{A short paragraph an AI coding agent could paste into its own context to build UI consistent with this system — restate the palette roles, font choices, and radius/spacing values as directives, e.g. "Use `var(--site-accent)` for primary CTAs, never hardcode hex."}}

<!-- provisional: review any section above marked with inferred (not explicitly stated) content -->
