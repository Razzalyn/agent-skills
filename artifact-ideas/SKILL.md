---
name: artifact-ideas
description: Brainstorm artifact ideas for the user's Trove using the Trove MCP. Surfaces work worth capturing, grounded in their existing artifacts and profile, and offers to create drafts directly.
---

# /artifact-ideas — Brainstorm artifacts for Trove

Help the user surface work that's worth capturing as a Trove artifact. The goal is to make it easy to remember and articulate accomplishments they'd otherwise forget — talks, shipped features, projects, side work, mentorship, writing, decisions, saves — and to drop the good ones into Trove as drafts in one step.

This skill assumes the Trove MCP is connected (tools prefixed `mcp__*__` with names like `get_my_profile`, `list_my_artifacts`, `create_draft_artifact`). If those tools aren't available, tell the user to connect the Trove MCP first (Settings → Connectors → `https://trove.co/api/mcp`) and stop.

## How to run

### 1. Ground in context first

Before brainstorming, call these in parallel:

- `get_my_profile` — name, role, onboarding status
- `list_my_artifacts` — existing artifacts so you can avoid duplicates and spot gaps

If the user gave you a hint in their message (e.g. "from the last quarter", "around the launch of X"), use it to focus. Otherwise default to the last ~3 months.

### 2. Ask one focused prompt, not a survey

After scanning their existing artifacts, ask **one** opening question tailored to what's missing. Don't enumerate a checklist — that's exhausting. Examples of good openers:

- "I see you've captured a lot of shipped features but nothing about mentoring or hiring. Anyone you've coached or interviews you've run recently that stuck with you?"
- "Your last artifact was 6 weeks ago. What's the most interesting thing you've worked on since then — even if it feels small?"
- "You have a few project artifacts but no talks or writing. Have you given any internal presentations, written a doc that got circulated, or led a working group?"

Stay conversational. One question, wait for an answer, follow up.

### 3. Probe for the artifact, not the activity

When the user mentions something, pull on the thread until you have enough to draft. Good artifacts have:

- **What happened** — concrete, specific
- **Why it mattered** — impact, audience, outcome
- **Your role** — what *you* did, not what the team did
- **When** — month + year if they remember

Follow-up examples: "Who was in the room?" / "What changed after?" / "How did you end up owning that?" / "What would have happened if you hadn't pushed on it?"

### 4. Categories to keep in your back pocket

If the conversation stalls, these tend to be under-captured:

| Category | Prompt |
|---|---|
| Talks & writing | Internal presentations, conference talks, blog posts, widely-shared docs |
| Saves | A time you caught something before it broke, or unblocked a stuck project |
| Decisions | A call you made (or pushed for) that shaped the direction of work |
| Mentorship & hiring | People you coached, interviews you ran, onboarding you led |
| Side work | OSS, advising, community, things outside your day job |
| Operational wins | Process changes, tooling, things that made the team faster |

Surface one category at a time, only when relevant.

### 5. Offer to draft — don't just summarize

Once you have enough for an artifact, **propose a draft** with a title and 4–8 sentence description in the user's voice. Then ask: "Want me to drop this into your Trove as a draft?" If yes, call `create_draft_artifact` with:

- `title` — short, headline-style
- `description` — the prose you drafted, in first person
- `type` — one of `talk`, `project`, `shipped feature`, `writing`, `mentorship`, etc., if obvious
- `artifactMonth` / `artifactYear` — only if both are known

Return the URL from the tool response so they can open it and add validators. Then ask if there's another one to capture.

## Style

- One question at a time, never a wall of bullets.
- Quote their words back when drafting — don't invent facts or inflate scope.
- If they're vague ("I worked on a project"), keep probing before drafting.
- Drafts are **drafts**: it's fine to capture something half-formed; they'll refine in Trove.
- Never submit, add validators, or claim to do anything the MCP can't actually do.
