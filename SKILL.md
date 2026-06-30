---
name: brand-interview
description: Runs a conversational brand-discovery interview to produce a structured brand profile (positioning, voice, audience, feeling, differentiation). Use this whenever the user wants to "do a brand interview," "figure out our brand voice," "define our positioning," "build a brand profile," or says something like "help me figure out what we stand for" / "I don't know how to describe my business." Works standalone, no external tools required — ask this skill to run even if a live brand-interview connector/MCP isn't available or has failed. Also use when the user wants to replicate, automate, or build a tool similar to a "brand interview" experience.
---

# Brand Interview

A guided, conversational interview that turns a business owner's half-formed sense of their brand into a structured brand profile: one question at a time, adapting when the person gets stuck, ending in a usable reference document.

## When to use this

- User wants to define brand voice, positioning, audience, or tone from scratch
- User says "I don't know" or goes blank on a brand question — this skill's job is to unstick them, not just collect an answer
- User wants the output turned into something reusable (a skill file, a style guide, brand brief)
- A live brand-interview MCP/connector is unavailable, errored out, or the user explicitly wants a standalone version

## Core interaction pattern

This is the most important part of the skill — it's not a form, it's a conversation.

1. **Ask one question at a time.** Never bundle multiple questions into one message. Wait for the answer before moving on.
2. **If the person answers directly and confidently** — accept it, optionally reflect it back in one tightened sentence, move to the next question.
3. **If the person says "I don't know," gives a one-word non-answer, or seems stuck:**
   - Don't repeat the question verbatim.
   - Offer 3-5 concrete, labeled directions specific to *their* business (not generic categories). Each option should be a short bolded label + a one-line gut-check phrase in quotes that someone might actually say or feel.
   - Ground the options in details you already know about their business from earlier answers — this is what makes the options feel specific instead of like a personality quiz.
   - End with an open invitation: "Which of these feels closest? Or is it something else?"
   - If they're still stuck after one round of options, narrow it further or just make a reasonable proposal and ask them to confirm/edit rather than asking open-ended again. Don't loop more than twice on the same question.
4. **Confirm before locking in an answer that came from your own suggestion**, not the user's original words. Restate it as the final phrasing you'll record, e.g. "Great — I'll record this as: '...'. Sound right?"
5. **Keep momentum.** No long preambles between questions. A short acknowledgment + the next question is usually enough.

## Question set

Ask these roughly in order, but skip/reorder/merge if the user's earlier answers already cover a later question. Don't recite this list to the user — ask one at a time, naturally.

1. **Core description** — "In one sentence, what does your business actually do?"
2. **Feeling** — "Describe the feeling someone should have working with you." (or: after they finish/use your product, what do they feel?)
3. **Audience** — "Who is the one person you most want to reach?" (push for a specific person/moment/struggle, not a demographic)
4. **Problem/stakes** — "What's the problem they have if they don't find you?"
5. **Differentiation** — "What do you do differently from everyone else in this space?"
6. **Proof** — "What's a moment — a result, a story, a transformation — that proves this works?"
7. **Voice** — "If your brand were a person talking, how would they sound? Casual, clinical, warm, direct, poetic...?"
8. **Things to avoid** — "Is there a tone, word, or vibe you definitely don't want associated with you?"
9. **Name/tagline check** (optional, only if relevant) — "Does your current name/tagline still feel right, or does it undersell what you just described?"

Adjust the question set for context — e.g., a wellness/retreat brand will lean into feeling and transformation; a B2B SaaS brand will lean harder into differentiation and proof.

## Handling "I don't know" — worked pattern

This is the technique to replicate, demonstrated:

> **Q: Describe the feeling someone should have working with you.**
> User: I don't know
>
> Response: Offer prompts grounded in what's already been said about the business (audience, problem, stakes), e.g.:
> - **Safe and held** — "I was scared going in, but I felt completely taken care of."
> - **Cracked open** — "Something in me finally moved."
> - **Coming home to themselves** — "I remembered who I actually am."
> - **Witnessed** — "Someone actually saw me."
>
> Then: "Which of those feels closest — or is it something else?"

Notice: each option is grounded in the specific business (a psychedelic retreat, in the worked example) — not a generic emotion wheel. Pull details from earlier answers to make the options land.

## Output: the brand profile

After the interview (or whenever the user wants to wrap up / has answered the core questions), produce a structured brand profile. Default to a markdown artifact unless the user asks for a different format (e.g. a Claude Code skill file, a docx).

Structure:

```markdown
# [Business Name] — Brand Profile

## What we do
[one-sentence description]

## The feeling we create
[feeling, with the supporting "why" — what it protects against or delivers]

## Who we're for
[specific audience description]

## The problem we solve
[stakes / what happens without us]

## What makes us different
[differentiation]

## Proof it works
[proof point / story]

## Voice
[tone descriptors + 2-3 concrete "sounds like" / "doesn't sound like" examples]

## Avoid
[words/tones/vibes to stay away from]
```

Keep each section tight — 1-3 sentences, not essays. This document should be usable as a drop-in reference for someone else writing copy for the brand.

## If asked to turn the output into a skill file

Some users (especially technical ones already using Claude Code) will want the brand profile turned into a reusable skill that future Claude sessions can load when writing copy for this brand. If asked:

- Use the skill-creator skill's format (`/mnt/skills/examples/skill-creator/SKILL.md`) as the structural reference
- `name`: the brand name, kebab-case, e.g. `ceremonia-brand-voice`
- `description`: should trigger whenever someone is writing copy, emails, web content, or any external-facing material for this brand
- Body: the brand profile content above, written as instructions ("When writing for [Brand], use this voice...") rather than as interview answers
- Offer to package it with `package_skill.py` if the user wants a downloadable `.skill` file

## Tone for running the interview

Warm, curious, a little informal — like a sharp creative director, not a survey tool. Short messages. No corporate filler ("Great question!", "I'd be happy to help you explore that..."). Get to the next useful thing quickly.
