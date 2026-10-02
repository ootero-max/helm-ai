---
description: Turn recorded meetings (Fathom) into next steps — my action items to todos, theirs to Waiting on, follow-up drafts for approval
allowed-tools: Read, Edit, Write, Bash(git add:*), Bash(git commit:*), Bash(date:*), Bash(mkdir:*)
argument-hint: [today | yesterday | <meeting title or attendee> | all]
---

You turn meetings into next steps. Input: `$ARGUMENTS`. Today is the current
date. Read `config.md` first (my name, team, boss). If Fathom tools are not
available, stop with one line: "Fathom isn't connected — connect it at
https://claude.ai/customize/connectors and run `/followup` again."

## Speed rules
- Read local files first, then issue every remote read in ONE parallel batch.
- Budget: **≤ 8 tool calls, ≤ 45 seconds.** One Fathom list call, then one
  detail call per meeting (max 5 meetings per run; say if more were skipped).
- Footer: `⏱ <elapsed>s · <n> tool calls`.

## Step 1 — Scope
Read `.helm/followup.md` if present: it lists meeting IDs already processed
and the `last_followup` timestamp.
- No argument → recorded meetings since `last_followup` (else last 48h),
  excluding processed IDs.
- `today` / `yesterday` → that day's recordings.
- `all` → last 7 days including processed (re-review).
- Anything else → the most recent recording whose title or attendees match.

## Step 2 — Gather (one batch)
From Fathom: the recordings in scope (title, time, attendees, duration), then
for each: the summary, action items, decisions, and any open questions. Do
not fetch transcripts unless the summary has no action items at all.
Also read `todos.md` so you can tell which action items are already captured
(match on wording / Jira key / meeting name).

## Step 3 — Report, one block per meeting
```
📼 <Title> — <date, time> · <duration> · <attendees>
Summary: <2–3 lines, decisions first>
Decisions: <one line each, or "none recorded">
Mine:      <action item> (due <date if stated>)  ← mark ✅ if already in todos.md
Theirs:    <action item> — <owner>
Open:      <question nobody answered>
```
Owner resolution: "I/me/Ozzy" → mine. Names from `config.md → Team` → theirs.
Unclear owner → list under Open with "(owner?)". Never invent a due date; use
one only if it was said in the meeting.

## Step 4 — Propose, then apply on approval
After all blocks, propose in one list, numbered:
- **Add to todos:** each of my uncaptured action items, with the bucket you'd
  pick (🔴 if my boss asked or a customer is waiting; 🟡 if someone needs a
  reply; else 🟢), `(due ...)` if stated, source `fathom <short title>`.
- **Waiting on:** each of theirs, with owner and `since` today.
- **Follow-up draft:** one per meeting if there were decisions or action
  items: a short Slack message (or Gmail draft for external attendees) to the
  attendees — decisions, who owns what, dates. 3–8 lines, my voice, no
  preamble. Drafts only, never send.
Ask: "Apply which? (numbers, `all`, or edits)". Then:
- Write the approved items to `todos.md` in the standard line format.
- Create approved drafts in Slack / Gmail (draft only).
- Write `.helm/followup.md`: `last_followup: <now>` and one line per
  processed meeting ID with title and date (keep the last 60 days).

## Step 5 — Commit
`git add todos.md .helm/followup.md && git commit -q -m "followup: $(date +%F)" || true`

Terse. Links where Fathom gives them. Personal meetings (no work attendees,
or titles like doctor/family) are skipped silently.
