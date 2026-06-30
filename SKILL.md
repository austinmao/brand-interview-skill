---
name: brand-interview
description: "Run a structured brand interview and produce a BRAND.md brand book"
version: "1.0.0"
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

Conducts a guided brand interview and assembles a complete `BRAND.md` — strategy, positioning, audience, narrative, and voice. Mirrors the clawinterview MCP protocol (`brand_next_question` → `brand_submit_answer` → `brand_finalize`) as a stateful conversational loop.

Excludes visual identity (→ `DESIGN.md`) and product content (→ `PRODUCT.md`).

## Phase Protocol

Mirrors the MCP flow. Run phases in order. Do not skip.

### Phase 0 — Source Intake
Ask: "Do you have an existing website, brand doc, or material to share first?"
- If yes: read it, extract signals, acknowledge 2–3 findings, continue to Phase 1 with those as pre-fills.
- If no: proceed directly to Phase 1.

Signals extracted here reduce questions in Phase 1 (mirrors `existing_websites` contract input).

### Phase 1 — Interview Loop (mirrors `brand_next_question` / `brand_submit_answer`)

Work through sections §0 → §10 in order. Load `references/interview-guide.md` for per-section questions and field definitions.

**Per-turn rules:**
- One question per turn. No lists of questions.
- Follow up once if answer is thin, vague, or clichéd.
- Max 2 follow-ups per field. After 2, infer + mark provisional, advance.
- After 2+ fields captured, reflect what you know before next question.
- Never suggest the answer in the question.
- Track section + field internally; never announce section transitions aloud.

**Checkpoints** (after each section completes):
- Briefly confirm what was captured: "Got it — let me note that before we move on."
- Do not produce a full summary — one line only.

**Claims gate** (FR-004/FR-005):
- If any answer mentions medical benefits, financial results, regulated substances, or legal outcomes → ask: "Do your claims require any compliance framing — disclaimers, safe-claim language, or regulated territory warnings?"
- Include §6 in output only if confirmed. Omit otherwise.

### Phase 2 — Candidate (simplified, mirrors `brand_select_candidate`)

After all sections complete, show the user a 3-line brand direction summary:
> "Here's what I'm about to assemble: [flag line] / [positioning category] / [topline tone]. Does this feel right, or is there anything to adjust before I produce the full document?"

Incorporate any last corrections, then proceed to Phase 3.

### Phase 3 — Finalize (mirrors `brand_finalize`)

Load `assets/brand-md-template.md`. Populate every field from the interview.

Mark any inferred (not explicitly stated) content with `<!-- provisional: review -->` immediately after the passage.

Produce the complete `BRAND.md` in a single fenced code block.

After the block, produce a **Provisional Summary**:
```
## Provisional Items (confirm before publishing)
1. [field] — [inferred value] — [why inferred]
```

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
