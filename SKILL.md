---
name: brand-interview
description: "Run a structured brand interview (StoryBrand, JTBD, archetype, positioning) and produce a BRAND.md brand book plus a DESIGN.md design system — auto-enhances if the Glance brand connector is installed"
version: "1.2.0"
permissions:
  filesystem: write
  network: false
triggers:
  - command: /brand-interview
metadata:
  openclaw:
    emoji: "🎯"
    requires:
      bins: []
      env: []
      os: ["darwin", "linux"]
---

# Brand Interview

Conducts a guided brand interview and assembles two complete documents — `BRAND.md` (strategy, positioning, audience, narrative, voice) and `DESIGN.md` (color, typography, spacing, and component tokens). Runs fully standalone in any Claude session. Automatically upgrades to a connected mode when the Glance brand connector's MCP tools are present (see **Mode Detection**) — no connector required to use this skill.

Excludes product content (→ `PRODUCT.md`).

Built on named brand-strategy frameworks, not ad hoc questions — see **Frameworks Applied**.

## Mode Detection (run first, every session)

Before Phase 0, check your available tools for `brand_next_question`, `brand_submit_answer`, `brand_generate_candidates`, `brand_select_candidate`, `brand_finalize`, `brand_finalize_design` (or a combined `brand_interview` tool). These exist only when the customer has the Glance brand connector installed and authorized.

- **Found → Connected mode.** Say once: "Found the Glance brand connector — I'll save your answers as we go, so this survives if we get disconnected." Follow the "Connected mode" notes inline in each phase below.
- **Not found → Standalone mode.** Say nothing extra. Proceed exactly as documented below — this is a first-class fully-featured path, not a degraded fallback.

If a connector call errors mid-session (`TENANT_UNREACHABLE`, `UNAUTHORIZED`, timeout, etc.): note once, plainly — "Connector call failed, continuing standalone for the rest of this session" — then drop to Standalone mode for everything after. Never surface raw error text or stack traces to the user, never retry silently forever.

## Phase Protocol

### Phase 0 — Source Intake
Ask: "Do you have an existing website, brand doc, or material to share first?"
- If yes: read it, extract signals, acknowledge 2–3 findings, continue to Phase 1 with those as pre-fills.
- If no: proceed directly to Phase 1.

Signals extracted here reduce questions in Phase 1.

### Phase 1 — Interview Loop

Work through sections §0 → §10 in order. Load `references/interview-guide.md` for per-section questions, field definitions, and the framework each field maps to.

**Per-turn rules:**
- One question per turn. No lists of questions.
- Follow up once if answer is thin, vague, or clichéd.
- Max 2 follow-ups per field. After 2, infer + mark provisional, advance.
- After 2+ fields captured, reflect what you know before next question.
- Never suggest the answer in the question.
- Track section + field internally; never announce section transitions aloud.

**Connected mode:** call `brand_next_question` for the next question instead of reading purely from the local guide — the connector may already know signals from the tenant's existing corpus and can shorten the interview. Submit each answer via `brand_submit_answer`. The connector returns text; you still apply every conversational rule above (one question per turn, follow-ups, no announcing sections). On error, fall back to the local guide for the rest of the session (see Mode Detection).

**Checkpoints** (after each section completes):
- Briefly confirm what was captured: "Got it — let me note that before we move on."
- Do not produce a full summary — one line only.

**Claims gate** (FR-004/FR-005):
- If any answer mentions medical benefits, financial results, regulated substances, or legal outcomes → ask: "Do your claims require any compliance framing — disclaimers, safe-claim language, or regulated territory warnings?"
- Include §6 in output only if confirmed. Omit otherwise.

### Phase 2 — Candidate Directions (A/B/C + recommendation)

After all sections complete, generate **3 labeled brand directions** from what was captured — not 3 different facts, 3 different ways to *frame* the same facts (different emphasis on positioning angle, tone register, or narrative frame). Each candidate:
- **Label** — 2–4 word name (e.g. "The Category Definer", "The Trusted Guide", "The Insurgent")
- **1–2 sentence rationale** — what this framing leans into and why it could fit
- Grounded in the user's own words only — no invented positioning

Present as:
> **A) [Label]** — [rationale]
> **B) [Label]** — [rationale]
> **C) [Label]** — [rationale]
>
> **Recommendation: [A/B/C]** — [1 sentence why, tied to what the user emphasized most]
>
> Pick one, blend two, or tell me what's off.

Fold the user's pick (or corrections) into the working brand model, then proceed to Phase 3.

**Connected mode:** this A/B/C step always runs locally — there is no MCP tool for brand-*strategy* candidates (`brand_generate_candidates` only produces visual/design-token directions; that step is Phase 4, not this one).

### Phase 3 — Finalize BRAND.md

Load `assets/brand-md-template.md`. Populate every field from the interview and the selected candidate framing.

Mark any inferred (not explicitly stated) content with `<!-- provisional: review -->` immediately after the passage.

Produce the complete `BRAND.md` in a single fenced code block.

After the block, produce a **Provisional Summary**:
```
## Provisional Items (confirm before publishing)
1. [field] — [inferred value] — [why inferred]
```

**Connected mode:** after producing the local `BRAND.md` block above, also call `brand_finalize` to persist the structured brief to the tenant's brand corpus (idempotent, retryable — safe to call even after a prior partial failure). Report plainly: "Saved to your brand connector" on `saved: true`; "Couldn't save to the connector this time — the copy above is still yours, try again later" on `saved: false`/`not_yet_saved`. Never claim it saved when it didn't.

### Phase 4 — Visual Candidate Directions (A/B/C + recommendation)

Using the §7 Visual Direction answers (`color_feeling`, `font_feel`, `shape`, `theme_mode`, `visual_direction`, `image_direction`, `inspiration_websites`) plus the finalized `BRAND.md`, generate **3 labeled visual/design-token directions** — different color palettes, typography pairings, and shape/spacing feels that all fit what the user described, not 3 unrelated aesthetics. Each candidate:
- **Label** — 2–4 word name (e.g. "Warm Minimal", "Bold Editorial", "Quiet Confidence")
- **Palette** — primary/accent/background/text hex values (pick real, specific hex values — never placeholders)
- **Type pairing** — a heading font + body font that fit the label
- **1 sentence rationale** — why this direction fits what the user said in §7

Present as:
> **A) [Label]** — [palette summary] / [type pairing] — [rationale]
> **B) [Label]** — [palette summary] / [type pairing] — [rationale]
> **C) [Label]** — [palette summary] / [type pairing] — [rationale]
>
> **Recommendation: [A/B/C]** — [1 sentence why]
>
> Pick one, blend two, or tell me what's off.

**Connected mode:** you MAY instead (or additionally) call `brand_generate_candidates` for real generated directions with live preview URLs, then `brand_select_candidate` on the user's pick — offer this as "I can also generate real visual design previews for these — want me to?" If the user declines or the call errors, fall back to the local A/B/C above. Either way, the winning direction's tokens (hex values, fonts, spacing/radius feel) feed Phase 5.

### Phase 5 — Design Assembly (DESIGN.md)

Load `assets/design-md-template.md`. Populate the frontmatter token block (`colors`, `typography`, `rounded`, `spacing`) from the Phase 4 winning direction, and write all 9 sections (`Visual Theme & Atmosphere`, `Color Palette & Roles`, `Typography Rules`, `Component Stylings`, `Layout Principles`, `Depth & Elevation`, `Do's and Don'ts`, `Responsive Behavior`, `Agent Prompt Guide`) — these exact headers, in this exact order.

Produce the complete `DESIGN.md` in a single fenced code block, immediately followed by a short `tokens.css` fenced block mapping every color/typography/radius/spacing value to a `--site-*` custom property (e.g. `--site-accent: {{HEX}};`) — never raw hex in the CSS selectors themselves, only in the `:root` variable declarations.

**Connected mode:** after producing the local `DESIGN.md` block above, also call `brand_finalize_design` to persist it. Same truthful reporting rule as Phase 3 — "Saved to your brand connector" only on confirmed success, never claim it saved when it didn't.

## Frameworks Applied

Named, established brand-strategy frameworks — declared here so customers know what they're getting, not hidden inside generic questions:

| Framework | Where it shows up |
|---|---|
| **StoryBrand** (Donald Miller) — Guide, Plan, Stakes | `guide_empathy`/`guide_authority` (§1), `plan[]` (§2), `stakes_success`/`stakes_failure` (§3) |
| **Jobs to Be Done (JTBD)** | `jtbd` (§3) — the functional/emotional/social job the customer hires the brand for |
| **Positioning statement** (category–differentiator–reason-to-believe) | `positioning_statement` (§2) — "For [audience], [brand] is the [category] that [differentiator] because [RTB]." |
| **Jungian / cultural brand archetype** | `archetype` (§3) |
| **Emotional arc mapping** | `emotional_sequence` (§1) — the state-to-state emotional journey a customer moves through |

Phase 4/5 (visual candidates → `DESIGN.md`) is a design-token generation step, not a named brand-strategy framework — it's listed here for completeness, not included in the table above.

## Inference Rule

When inferring provisional values: derive from the user's own words and industry context only. Never borrow language, metaphors, or themes from any example brand. If you catch yourself writing words the user never used — mark the field provisional instead.

## Quality Gate (self-check before output)

- All required sections present: §0, §1, §2, §3, §4, §5, §7, §9, §10
- §6 included ONLY if claims gate triggered
- No product features/pricing (→ PRODUCT.md)
- Every inferred field marked `<!-- provisional: review -->`
- `words_to_avoid[]` populated (feeds banned-word kill list)
- `positioning_statement` uses exact format: "For [audience], [brand] is the [category] that [differentiator] because [RTB]."
- Values each have name + brand-specific meaning (not generic platitudes)
- `plan[]` has exactly 3 steps, each 2–4 words (StoryBrand Plan)
- `guide_empathy` + `guide_authority` both present in §1 (StoryBrand Guide)
- `stakes_success` + `stakes_failure` both present in §3 (StoryBrand Stakes)
- `jtbd` present in §3
- §7 has all 7 visual direction fields — these are required for the design-token pipeline
- Phase 2 produced exactly 3 labeled candidates + 1 recommendation before Phase 3 ran
- Phase 4 produced exactly 3 labeled visual candidates + 1 recommendation before Phase 5 ran
- `DESIGN.md` frontmatter has all 4 token groups (`colors`, `typography`, `rounded`, `spacing`) fully populated, no `{{PLACEHOLDER}}` left unfilled
- `DESIGN.md` has all 9 required section headers, verbatim and in order
- `tokens.css` uses `--site-*` custom properties only — no raw hex outside the `:root` declarations
