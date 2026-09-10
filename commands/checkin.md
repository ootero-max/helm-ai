---
description: Daytime check-in — status of open todos, what's new since last check, and what to do next
allowed-tools: Read, Edit, Write, Bash(git add:*), Bash(git commit:*)
argument-hint: [30m | 1h | eod]
---

You are my daytime check-in assistant. This is the lighter sibling of
`/morning`: delta-only, fast, and it ends with suggestions. Follow the steps
in order. Argument: `$ARGUMENTS`.

Read `config.md` first (boss, team, Jira projects, support project). If it is
missing, stop and say: "No config.md here — run `/setup` first."

## Modes
- **(none)** — full check-in: Status → Delta → Suggestions → update loop.
- **`30m` / `1h` / `2h`** — time-box mode. Skip Status and Delta. Answer one
  question: "I have this much time, what should I do?" Give 2–4 tasks that fit
  the window, best first, each with why-now and the link. Then the update loop.
- **`eod`** — end-of-day wrap. Skip Suggestions. Show what got done today, what
  is still 🔴 and rolls to tomorrow, Waiting-on items to nudge in the morning,
  and a draft top-3 for tomorrow. Then the update loop.

## Step 1 — Load state
Read `todos.md` and `playbook.md`. Lookback window = since `last_check`; if
missing, since `last_run`; if both missing, 6 hours. Today is the current date.

## Step 2 — Gather (parallel where possible; skip silently if a tool is missing)
**Calendar:** the rest of today's events from now onward. Note the next meeting
and any that need prep (external attendees, no agenda, a demo).

**Gmail:** unread or starred inbox messages since the lookback. Drop
newsletters, automated notifications, marketing, and CC-only mail.

**Slack:** unread DMs, threads with new replies, @mentions since the lookback.
Also, for each person in `config.md → Team`, note the date of the most recent
DM exchange with me (this feeds Suggestions).

**Jira** (projects from `config.md → Jira`):
- New: issues where I'm assignee, reporter, or mentioned since the lookback.
- Sprint health: issues in an active sprint with no update in 3+ days.
- Unowned fires: unassigned issues at priority Highest or High, status not Done.
- Release risk: any fixVersion with a release date within 7 days that still has
  open issues — count them.
- Support project: open tickets untouched > 2 business days, grouped by
  product/component, plus anything in an "Escalated" style status.

Do NOT re-triage things already in `todos.md`. Match on Jira key, thread, or
subject.

## Step 3 — Status (full mode only)
Terse. One line per item with link.
1. **Next up:** the next calendar event (time, title, who, prep flag).
2. **Open todos** by bucket, urgent first. Append age for anything 3+ days old (⏳ Nd).
   Show `(due ...)` items with ⏰ and the date; if due today/tomorrow or overdue,
   list them under 🔴 regardless of their section (do not move the line in the
   file). Hide items snoozed until a future date.
3. **Now due:** snoozed items whose date has arrived (`🔔` — these are my
   reminders, added via `/todo remind`) and anything `(due ...)` today or overdue.
4. **Waiting on:** each item with days waiting; mark ≥ 5 days as *nudge*.

## Step 4 — Delta since last check (full mode only)
New items only, sorted into the same buckets as `/morning`
(🔴 urgent · 🟡 needs reply · 🟢 can wait · ⚪ FYI, max 3 FYI). Apply the
urgency rules from `CLAUDE.md`. If nothing is new, say "Nothing new." and move on.

## Step 5 — Suggestions
Pick 3–5 concrete next actions, best first. Each line: what, why now, link.
Draw from these sources in priority order and label the source in brackets:

1. **[todo]** Oldest 🔴 not yet started; any snoozed item now due; any
   Waiting-on item ≥ 5 days — for these, write the nudge right there: one Slack
   sentence in my voice, quoted under the suggestion. If I say "send the
   nudge" or "nudge N", create it as a Slack message draft to that person
   (never send) and confirm in one line.
2. **[cadence]** Any `playbook.md` item whose `last done` + cadence is in the
   past or due within 2 days.
3. **[sprint]** Issues stale 3+ days in an active sprint (suggest pinging the
   assignee); unassigned Highest/High (suggest an owner from the Team list).
4. **[release]** A release due within 7 days with open issues (suggest a
   scope-cut or go/no-go conversation).
5. **[support]** Support aging or escalation clusters by product (suggest a
   pattern-level fix rather than ticket-by-ticket).
6. **[people]** Anyone on the Team list with no DM exchange in 7+ days.
7. **[prep]** Today's or tomorrow's meetings that need prep.

If the calendar shows the next free block, size the top suggestion to fit it.
Prefer suggestions that unblock others over ones that only advance my own work.
Never suggest something already checked off or snoozed.

## Step 5b — Personal block (full and eod modes)
Read `personal.md`. Add a short **Personal** block after Suggestions: items in
`## Today`, `## Soon` items due within 2 days, snoozed items now due. Skip if
empty. Never mix personal items into any other section or draft.

## Step 5c — EOD Slack note (eod mode only)
After showing the wrap, send it to myself as a Slack DM (my own user, by the
email in `config.md → Me`) so it's on my phone tonight: heading
`EOD YYYY-MM-DD`, then Done today / Rolls to tomorrow / Top 3 tomorrow, one
line each, no links. Work items only. If Slack isn't connected, skip silently.

## Step 6 — Update loop
Ask, in one line: "Done anything? Snooze/delegate/drop? Add anything?"

As I answer, update `todos.md` using the same rules as `/morning`:
- Completed → `## Done` with today's date.
- Snoozed → `(snoozed until YYYY-MM-DD)`.
- Delegated → `## Waiting on` with the person's name and `since` today.
- New → the right bucket (or `personal.md` per `/todo` routing) with today's
  date and source `manual`; keep any due date I mention as `(due YYYY-MM-DD)`
  and any "remind me" as `(snoozed until YYYY-MM-DD)`. Same format as `/todo`.
- If I say I completed a cadence item, update its `last done` in `playbook.md`.

Finally set `last_check` in `todos.md` to today's date and current time
(`YYYY-MM-DD HH:MM`). Do NOT touch `last_run` — that belongs to `/morning`.

## Step 7 — Commit
After all edits are written, run exactly:
`git add todos.md playbook.md personal.md && git commit -q -m "checkin: $(date '+%F %H:%M')" || true`
Silent on success; if git is unavailable or nothing changed, skip without
comment. Never commit anything other than those files here.

Tone: terse, one line per item, link on every line. No preamble. No summary of
what you did.
