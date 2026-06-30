# BRAND.md Contract

Defines the spec-178 BRAND.md schema. This is the typed output contract that `brand_finalize` (MCP) or Phase 3 (skill) must satisfy.

## Frontmatter (machine-readable, required)

```yaml
---
brand: "Brand Name"
tagline: "Flag line"
version: "1.0.0"
language: "en"
tenant: "slug-lowercase-hyphenated"
generated: "YYYY-MM-DD"
generator: "brand-interview-skill"
---
```

All keys required. Unknown extra keys allowed. Downstream tools parse frontmatter without reading body prose.

## Section Contract

Required H2 headers, exact text, exact order:

```
## §0 Brand in 60 Seconds
## §1 Brand Foundation
## §2 Positioning
## §3 Audience
## §4 Brand Narrative
## §5 Verbal Identity & Voice
## §6 Claims & Compliance        ← ONLY when claims gate = include
## §7 Visual Direction            ← feeds design-token pipeline
## §9 Brand Architecture
## §10 Governance
```

§8 intentionally absent (reserved: product → PRODUCT.md).

## Field Registry

### §0 fields
- `flag_line` — str, short
- `promise` — str, phrase
- `mechanism` — str, phrase
- `credibility` — str, 2–4 bullet points
- `proof` — str, concrete numbers/facts
- `primary_cta` — str, 3–5 words
- `one_line_statement` — str, max 30 words
- `north_star` — str, 1–2 sentences

### §1 fields
- `purpose` — str
- `mission` — str
- `vision` — str
- `enemy` — list[str], 2–5 items
- `aspirational_identity` — str
- `emotional_sequence` — str, arrow-linked states: `A → B → C`
- `values` — list[{name: str, meaning: str}], 3–6 items
- `guide_empathy` — str, StoryBrand Guide: how brand demonstrates it understands the customer's pain
- `guide_authority` — str, StoryBrand Guide: credentials/proof/track record qualifying the brand to lead

### §2 fields
- `category` — str
- `core_differentiator` — str
- `secondary_differentiators` — list[str]
- `what_we_are_not` — list[str]
- `positioning_statement` — str, MUST follow: "For [audience], [brand] is the [category] that [differentiator] because [RTB]."
- `plan` — list[str], StoryBrand Plan: exactly 3 steps, each 2–4 words

### §3 fields
- `primary_audience` — str, psychographic portrait
- `emotional_reality` — str
- `what_they_want` — list[str]
- `what_they_fear` — list[str]
- `archetype` — str
- `jtbd` — str, Jobs to Be Done: functional + emotional + social job dimensions
- `stakes_success` — str, StoryBrand Stakes: vivid picture of customer life after success
- `stakes_failure` — str, StoryBrand Stakes: what's lost or stays broken without action

### §4 fields
- `narrative` — str, long-form, 150–400 words

### §5 fields
- `topline_tone` — list[str], 3–5 adjectives
- `foundation_tone` — list[str], underlying qualities
- `guardrails` — list[str], anti-voice (never sound like)
- `writing_principles` — list[str], 4–7 rules
- `message_pillars` — list[str], 3–5 themes
- `words_to_own` — list[str]
- `words_use_carefully` — list[str]
- `words_to_avoid` — list[str] ← passthrough to banned-word kill list
- `slogans` — list[str]
- `this_not_that` — list[{this: str, not_that: str}]
- `house_style` — str

### §6 fields (conditional)
- `safe_claim_territory` — list[str]
- `claims_to_avoid` — list[str]
- `framing_rule` — str

### §7 fields (visual direction — feeds design-token pipeline)
- `color_feeling` — str, sensory/emotional color description
- `font_feel` — str, typographic mood
- `shape` — str, geometry and edge quality
- `theme_mode` — enum: `light` | `dark` | `both`
- `visual_direction` — str, 2–3 sentence aesthetic summary
- `image_direction` — str, photography vs illustration + style notes
- `inspiration_websites` — list[str], URLs (passed to design-token pipeline, not fetched here)

### §9 fields
- `master_brand` — str
- `founder_role` — str
- `architecture_rule` — str

### §10 fields
- `preapproval_checklist` — list[str]

## Provisional Marker

Any inferred (not operator-confirmed) content MUST be marked:

```
**Promise:** [inferred value] <!-- provisional: review -->
```

Marker is inline, after the content, on the same line or block. Human-visible in raw markdown. Machine-greppable. Invisible when rendered.

Operator-confirmed answers NEVER get the marker. Source-extracted content that was not confirmed by the operator DOES get the marker.

## Invariants

- Structurally complete even when near-empty — every required section header present; missing values inferred+provisional, never omitted headers
- §6 present iff claims gate = include; absent otherwise
- No visual identity fields anywhere (no colors, fonts, logos)
- No product fields anywhere (no features, pricing, specs)
- Template identical across tenants — every brand starts from the same structure
- Re-assembling with same inputs produces identical output
