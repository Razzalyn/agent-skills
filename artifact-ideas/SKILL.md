---
name: artifact-ideas
description: Brainstorm artifact ideas for the user's Trove using the Trove MCP. Surfaces work worth capturing, grounded in their existing artifacts and profile, and offers to create drafts directly. Runs interactively in a chat, or unattended as a weekly scheduled task.
---

# /artifact-ideas — Brainstorm artifacts for Trove

Help the user surface work that's worth capturing as a Trove artifact. The goal is to make it easy to remember and articulate accomplishments they'd otherwise forget — talks, shipped features, projects, side work, mentorship, writing, decisions, saves — and to drop the good ones into Trove as drafts in one step.

This skill assumes the Trove MCP is connected (tools prefixed `mcp__*__` with names like `get_my_profile`, `list_my_artifacts`, `create_draft_artifact`). If those tools aren't available, tell the user to connect Trove first — one click from [Trove in Claude's connector directory](https://claude.ai/directory/connectors/trove), or paste `https://thetrove.io/api/mcp` into the app's custom-connector / MCP server settings — and stop.

Two ways to run:

- **Interactive** (default): the user is in the chat. Follow "How to run" below — one question at a time, draft together, ask before saving.
- **Unattended**: nobody will answer. This is how the skill runs from a weekly scheduled task. Follow "Weekly, unattended mode" at the end instead: no questions, a short list of proposals, at most one draft.

Treat a run as unattended when the message says so ("weekly", "scheduled", "unattended", "don't ask me anything"), or when it arrives from a scheduled task rather than a person typing.

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

## Weekly, unattended mode

A scheduled task runs this skill with nobody on the other end, so the interview above can't happen. The job is to leave the user a short, specific note they can act on in two minutes — not to invent artifacts.

1. **Ground in context** exactly as in step 1: `get_my_profile` and `list_my_artifacts` (ask for `status: "draft"` as well as published, so you don't propose something that already sits in their drafts). If the task's message names other sources you may read (calendar, email, GitHub, Slack), scan the last seven days there for finished work: a shipped feature, a talk, a doc that circulated, a decision, a save. Don't go further back than the previous run would have.
2. **Propose three to five ideas**, each as one line: a headline-style title, the month, and the gap it fills in their Trove (the first mentorship artifact, nothing captured since July, no writing yet). Lead with the ones you have evidence for. If the profile and artifacts are all you have, propose from the under-captured categories in step 4 and say that's what you did.
3. **Draft at most one**, and only when you have enough real material to write all three STAR elements (situation, action, result) from things the user actually did — never from a guess. Call `create_draft_artifact` with it, return the URL, and name which elements are thin so they can finish it in Trove. A draft is private until the user publishes it, so a thin draft is fine; a fabricated one is not. With no such material, create nothing and say so.
4. **End with one question** for the user to answer next time they're in the chat: which idea to draft, or what the week's standout piece of work was. Keep the whole message under ~200 words.

Never ask a question and wait for it, never create more than one draft per run, and never repeat an idea you already proposed in a previous run that the user didn't act on — if you can see your past runs, treat those as declined.

### Setting it up as a weekly routine (what to tell the user)

If the user asks how to make this recur, give them these steps:

1. Connect Trove to Claude (link above) if it isn't already.
2. In Claude, open **Scheduled tasks** and create a new task. Pick a weekly time (Friday afternoon or Monday morning works well).
3. Paste this as the task's prompt, and enable the Trove connector (plus any others they want scanned):

   > Run the Trove artifact-ideas skill in weekly, unattended mode. Review my Trove profile and artifacts, propose three to five artifact ideas from the past week, create at most one draft if you have real material for it, and end with one question for me.

4. Save. Each run leaves a short message in Claude; drafts it creates appear under their Trove drafts, private until they publish.

If the user's Claude also exposes Trove's own prompts (the MCP server publishes this skill as the `artifact_ideas` prompt), they can pick it from the prompt menu instead of pasting text.
