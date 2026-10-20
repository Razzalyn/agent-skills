---
name: artifact-from-source
description: Turn something the user just finished — a pull request, a document, a deck, meeting notes, a published post — into a Trove artifact draft. Reads the source first, drafts in the user's voice, and asks only for what the source can't show. Uses the Trove MCP.
---

# /artifact-from-source — Draft a Trove artifact from a thing you made

The user has a concrete piece of work in hand ("make an artifact from this PR", "capture the deck I presented yesterday", a pasted link or excerpt). Your job is to read it, pull out the accomplishment, and leave a private draft in their Trove that they can finish and publish.

This skill assumes the Trove MCP is connected (tools prefixed `mcp__*__` with names like `get_my_profile`, `list_my_artifacts`, `create_draft_artifact`). If those tools aren't available, tell the user to connect Trove first — one click from [Trove in Claude's connector directory](https://claude.ai/directory/connectors/trove), or paste `https://thetrove.io/api/mcp` into the app's custom-connector / MCP server settings — and stop.

## How to run

### 1. Read the source before you say anything

Fetch it with whatever you have: a URL fetch, the GitHub connector for a PR or issue, Drive or Notion for a doc, a meeting-notes connector for a transcript, or the text the user pasted. Read the whole thing, not the title.

If you can't read it (private repo, no connector, a login wall), say exactly that and ask the user to paste the relevant part. Never draft from a URL you couldn't open.

### 2. Ground in the user's Trove

Call `get_my_profile` and `list_my_artifacts` (drafts as well as published). If an existing title clearly covers this same work, say so and ask whether they want a second artifact or to finish the one they have.

### 3. Pull the artifact out of the source

Take notes, in these four buckets, quoting the source where you can:

| Bucket | Where it usually hides |
|---|---|
| **Situation / task** — what was broken, missing or at stake before | PR description's "why", a doc's problem statement, the first slide, the meeting's agenda item |
| **Action** — what *the user* did, as distinct from the team | Commit authorship, who wrote the doc, who presented; the decisions and trade-offs the text records |
| **Result** — what changed | Merged-and-shipped, metrics in the description or follow-up comments, decisions taken in the meeting, reach of the post |
| **When** | Merge date, doc's last edit, event date, publish date |

Keep two lists: what the source *shows* and what it *doesn't*. Most sources show the situation and the action well and the result poorly.

### 4. Ask only about the gaps — two questions at most

Ask about whichever bucket the source left thin, one question at a time, in your own words. The result is the usual gap: "the PR merged — what did it change for users or the team, and is there a number?" Push for specifics when the answer is vague. If the user says to just save it, save it.

### 5. Keep it publishable

An artifact is private as a draft but public once the user publishes it. Leave out anything the source treats as internal: unreleased product names, customer names unless the source is already public, internal URLs, security details, dollar figures that read as confidential. Describe the shape of the work instead ("a large enterprise customer", "an internal admin tool"). If the whole thing can't be told publicly, say so before drafting.

### 6. Draft, then offer to save

Write the draft the way Trove expects: one STAR story in the user's first-person voice, two to four short paragraphs, plain prose, concrete verbs, the user's contribution distinguished from the team's, a result with a number when the source or the user gave one. Quote the user's and the source's words; don't inflate scope.

Propose:

- **Title** — headline-style, what was done, not the artifact name ("Cut checkout latency 40% by moving fraud checks off the request path", not "Fraud-check PR")
- **Description** — the draft
- **Type** — from the source kind: PR or release → `shipped feature`; design doc or RFC → `writing` or `decision`; deck or recording → `talk`; a post → `writing`; a project retro → `project`
- **When** — month and year from the source's dates, only if both are clear

Ask "Want this in your Trove as a draft?" If yes, call `create_draft_artifact` with `title`, `description`, `type`, and `artifactMonth` / `artifactYear` (both or neither). Give the user the URL it returns.

Then, in one line, name who in the source could validate it (the reviewers on the PR, the people in the meeting, the co-author). Trove's tools can't add validators, so the user does that in Trove; you're just saving them the thinking.

## Style

- Read first, talk second. The user shouldn't have to re-explain what the source already says.
- Two questions, not a survey. Stop as soon as all three STAR elements come from the source or the user.
- Never submit, publish, add validators, or claim to do anything the MCP can't actually do.
- Never paste secrets or internal-only details into the draft.
