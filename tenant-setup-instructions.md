# Brand Interview — Setup Instructions

You're setting up an AI-guided brand interview that will produce a complete brand book (`BRAND.md`) and a design system (`DESIGN.md`) for your business.

The interview covers: your brand promise, positioning, audience, story, and voice guidelines. At the end you get a structured document you can share with your team, use to brief copywriters, and feed into your design and content workflows.

It takes 20–40 minutes. No design or marketing experience required.

---

## Option A — Claude.ai (recommended)

Claude produces a formatted, editable document at the end. Best experience.

**Setup (one time):**

1. Go to [claude.ai](https://claude.ai) and sign in.
2. In the left sidebar, click **Projects** → **New Project**.
3. Name it `Brand Interview`.
4. Click **Add content** → **Upload files**.
5. Upload the file: `brand-interview-full.md` (provided alongside these instructions).
6. Click **Start conversation** inside the project.

**Running the interview:**

1. Open a conversation inside the Brand Interview project.
2. Type: `start the brand interview`
3. Claude will greet you and ask if you have any existing brand material to share first (website, old brand doc, etc.).
4. Answer one question at a time. There are no right or wrong answers — just be honest about your business.
5. The interview takes 20–40 minutes depending on how much you want to share.

**At the end:**

Claude will produce your `BRAND.md` as a formatted document in the right panel (the "Artifact" panel). You can:
- Copy it to a Google Doc or Notion page
- Download it as a markdown file
- Edit it directly in the panel

Review any item marked `<!-- provisional: review -->` — these are places where Claude made a reasonable guess and needs your confirmation.

---

## Option B — ChatGPT

**Setup (one time):**

1. Go to [chatgpt.com](https://chatgpt.com) and sign in.
2. Click **Explore GPTs** → **Create a GPT**.
3. Click the **Configure** tab.
4. Set **Name**: `Brand Interview`
5. Set **Description**: `Guides you through a structured brand interview and produces a complete brand book.`
6. Paste the entire contents of `brand-interview-full.md` into the **Instructions** field.
7. Under **Capabilities**, disable Web Browsing, Image Generation, and Code Interpreter.
8. Click **Save** → **Only me** (or share as you prefer).

**Running the interview:**

1. Open your Brand Interview GPT.
2. Type: `start the brand interview`
3. Follow the same process as Option A above.

**At the end:** The output appears as a copyable code block. Copy it into your doc tool of choice.

---

## Option C — GitHub (Claude Code, or claude.ai Skills)

If your team uses Claude Code, or claude.ai's Skills feature, you can pull this skill directly instead of copy-pasting a file.

1. Repo: [github.com/austinmao/brand-interview-skill](https://github.com/austinmao/brand-interview-skill)
2. **Claude Code:** clone it into your project's `.claude/skills/brand-interview/` (or anywhere Claude Code looks for skills), then run `/brand-interview`.
3. **claude.ai Skills:** download the repo as a zip (Code → Download ZIP), then in claude.ai go to Settings → Capabilities → Skills → Upload skill, and select the unzipped folder.
4. Either way — no separate paste step needed. Same interview, same `BRAND.md` + `DESIGN.md` output as Options A/B.

---

## Tips for a good interview

- **Share existing material upfront.** If you have a website, old pitch deck, or any brand doc — paste it or share the URL at the start. Claude will read it and ask fewer questions.
- **Be specific, not aspirational.** "We help burned-out executives find stillness" beats "we help people live better lives."
- **Real examples beat descriptions.** When asked about voice or customers, share something you actually wrote, or describe an actual customer by name.
- **It's okay to say "I don't know."** Claude will make a reasonable guess and mark it for your review.
- **You can pause and resume.** If you run out of time, save the conversation and continue in a new message.

---

## What the brand book is (and isn't)

**Included:**
- Brand promise, mission, vision, values
- Positioning and differentiation
- Audience psychographics
- Brand narrative (your story)
- Voice guidelines, tone rules, banned words
- Visual direction — colors, typography, spacing, and component tokens (`DESIGN.md`)

**Not included** (this lives in a separate document):
- Product features, pricing, specs — those go in your Product doc (`PRODUCT.md`)

---

## Questions?

Contact your account manager or reply to the onboarding email.
