---
description: One-screen prep brief for a meeting — attendees, recent exchanges, their open Jira, related todos
allowed-tools: Read, Edit, Write
argument-hint: [next | <meeting name or attendee> | tomorrow]
---

You prepare me for a meeting. Input: `$ARGUMENTS`. Today is the current date.
Read `config.md` first (team, Jira projects, support project, company).

## Step 1 — Pick the meeting
- `next` or empty → the next calendar event from now that has at least one
  other attendee.
- `tomorrow` → every meeting tomorrow with an external attendee or a Team
  member; produce a short brief for each.
- Otherwise → the soonest event whose title or attendee list matches the text.
If no calendar tool is connected, ask me for attendees and topic in one line.

## Step 2 — Gather (parallel; skip silently if a tool is missing)
For each attendee other than me:
- **Who:** name, role if known (Team list in `config.md`, else email domain →
  customer / partner / vendor / internal).
- **Gmail:** the most recent thread with them in the last 30 days — subject,
  last sender, open question if any.
- **Slack:** the last DM exchange or thread with them — date and gist.
- **Jira:** open issues where they are assignee or reporter, sorted by
  priority; anything stale 3+ days; anything in the support project if they
  are a customer.
- **Todos:** every line in `todos.md` that mentions them, including Waiting-on.
- **Intercom** (customers only, if connected): open conversations for their
  company in the last 30 days.
Also read the calendar event description and any linked doc title.

## Step 3 — Brief
One screen, nothing more. Sections in this order, one line per bullet, links
on every line:
1. **Meeting** — time, title, duration, location/link, agenda if any.
2. **People** — one line each: who they are + the last thing between you.
3. **Open threads** — what they're waiting on from me; what I'm waiting on from them.
4. **Their Jira** — top 3–5 items, priority-ordered, stale flagged.
5. **Likely to come up** — 2–4 bullets inferred from the above. Be specific.
6. **Suggested asks** — 1–3 things I should get out of this meeting.
If the meeting is a 1:1 with a Team member, stop and run `/1on1 <name>` instead.

Terse. No preamble. Don't edit any files. Personal items in `personal.md` are
never included.
