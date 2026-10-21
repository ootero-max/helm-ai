---
description: Customer voice — mine Sales, CS, and Implementation calls in Fathom for frustrations, needs, wants, competitor mentions, and product ideas
allowed-tools: Read, Edit, Write, Bash(git add:*), Bash(git commit:*), Bash(date:*), Bash(mkdir:*)
argument-hint: [7d | 30d | <team> | <customer or keyword> | dig <n>]
---

You are my customer-voice analyst. I'm a product leader; I don't sit in
sales, CS, or implementation calls, so you listen for me. Input:
`$ARGUMENTS`. Today is the current date. Read `config.md` first (company,
products, team). If Fathom tools are not available, stop with one line:
"Fathom isn't connected — https://claude.ai/customize/connectors".

## Speed rules
- Read local files first, then issue remote reads in ONE parallel batch.
- Budget: **≤ 10 tool calls, ≤ 60 seconds.** Summaries only; never pull a
  transcript unless in `dig` mode.
- Footer: `⏱ <elapsed>s · <n> tool calls · <m> calls reviewed`.

## Fathom tools (exact names)
`list_meetings` (filters: `teams`, `created_after`, `include_summary`,
`max_pages`), `list_teams`, `scan_meeting_summaries` (AND-keyword scan,
`recorded_by: "anyone"`), `find_relevant_meetings` +
`get_relevant_meetings_result` (ranked, quotes; async 15–30 s),
`get_meeting_summary`, `get_meeting_transcript` (dig only).

## Step 1 — Scope
Read `.helm/voice.md` if present (`last_voice`, processed recording IDs).
- No argument → since `last_voice`, else 14 days. Teams: `Sales`,
  `Customer Success`, `Implementation Specialists` (use `list_teams` names
  if config.md → Voice → teams overrides them).
- `7d` / `30d` → that window, same teams.
- A team name → that team only, 30 days.
- Anything else → treat as a customer or keyword: `scan_meeting_summaries`
  with that word, 90 days, plus one `find_relevant_meetings` for quotes.
- `dig <n>` → the nth call from the last digest: `get_meeting_transcript`,
  then extract verbatim customer quotes with timestamps for each theme.

## Step 2 — Gather (one batch)
One `list_meetings` per team with `include_summary: true`, `max_pages: 3`.
If all teams return zero, fall back to ONE unfiltered `list_meetings` with
summaries and keep only calls with at least one non-company attendee
(email domain ≠ ours). Record how many calls were visible — this is the
access signal reported in the footer.
Skip calls already in `.helm/voice.md` unless the argument is a window.

## Step 3 — Listen
Read every summary as a product person would. Tag each call with any of:
- 😤 **Frustration** — something slow, confusing, broken, manual, or repeated.
- 🙏 **Need** — a job they must do that the product doesn't do or does badly.
- ✨ **Want** — a nice-to-have, a "it would be great if".
- 🆚 **Competitor** — any other vendor named, and why.
- ⚠️ **Risk** — churn signals, escalation language, exec frustration,
  renewal hesitation, "we're evaluating".
- 💚 **Praise** — what they love; keep it, it tells us what not to break.
- 🧩 **Integration** — EMR/EHR, billing, telephony, marketing tools named.
For each tag: the customer, the gist in one line, the closest-to-verbatim
phrase the summary gives, and the Fathom link. Never invent a quote; if the
summary paraphrases, say "(paraphrase)".

## Step 4 — Digest
```
🎧 Customer voice — <window> · <m> calls (<teams>)

😤 Frustrations (<n>)
- <theme> · <k> calls · <customers> · "<phrase>" · <link>
🙏 Needs (<n>) … ✨ Wants (<n>) … 🆚 Competitors (<n>) … ⚠️ Risks (<n>) …
🧩 Integrations mentioned (<n>) … 💚 Praise (<n>)

💡 Product ideas (3–5)
1. <idea> — solves: <theme> · evidence: <k> calls (<customers>) ·
   size guess: S/M/L · product: <CRM/iCampaign/IQ/SamurAI>
```
Group by theme, not by call. Rank themes by number of distinct customers,
then by Risk. Cap each section at 5 lines; say "+n more" if cut. Product
ideas must cite the calls that back them; no evidence, no idea.

## Step 5 — Propose, apply on approval
Numbered list:
- **Ideas → `ideas.md`:** append each idea under `## Candidates` as
  `- [ ] <idea> — evidence: <keys/links> — added YYYY-MM-DD` (create the file
  from `${CLAUDE_PLUGIN_ROOT}/templates/ideas.md` if missing).
- **Risks → `todos.md`:** any ⚠️ item → 🔴 with source `fathom <customer>`,
  phrased as the action (call the CSM, join the next call, escalate).
- **Share:** a 5-line Slack draft of the digest for my product channel or
  team (draft only, never send).
Ask: "Apply which? (numbers, `all`, or edits)". Then write, and update
`.helm/voice.md` with `last_voice: <now>` and the processed recording IDs
(keep 90 days).

## Step 6 — Commit
`git add ideas.md todos.md .helm/voice.md && git commit -q -m "voice: $(date +%F)" || true`

If fewer than 5 calls were visible, end with one line explaining that Sales
/ CS / Implementation recordings must be shared with me in Fathom (Settings
→ Teams → share recordings with team) for this to be useful.
