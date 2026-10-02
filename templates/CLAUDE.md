# Helm AI workspace

This folder is my personal Helm AI workspace. Commands come from the `helm-ai`
plugin; my data lives here. Who I am, my boss, my team, my Jira projects, and
my preferences are in `config.md`. Read it before doing anything else.

## Commands (all from the helm-ai plugin; `.claude/commands/` holds thin aliases)
- `/morning` — full triage before the day starts. Owns `last_run`.
- `/checkin` — daytime delta + suggestions. Owns `last_check`.
  Modes: `/checkin`, `/checkin 30m`, `/checkin eod`.
- `/todo` — instant capture. Add, due date, reminder, done, snooze, drop, list.
  Personal items go to `personal.md`.
- `/draft` — reply drafts for the 🟡 bucket. Never sends.
- `/prep [next|<meeting>|tomorrow]` — one-screen meeting brief.
- `/weekly` — weekly review + draft update to my boss.
- `/1on1 <name>` — prep for a direct report; notes in `1on1/<name>.md`.
- `/followup` — Fathom recordings → my action items to todos, theirs to
  Waiting on, follow-up drafts. Tracks processed meetings in `.helm/followup.md`.
- `/setup [section]` — first-run configuration, or redo one section.
- Every command commits what it changed; nothing pushes. `git push` when I want.

## Urgency rules
1. Anything from my boss (config.md → Boss) is 🔴 by default.
2. Customer-facing escalations and support tickets aging > 2 business days are 🔴.
3. A blocked engineer or blocked release is 🔴.
4. Review/approval requests are 🟡 unless they block someone (then 🔴).
5. Newsletters, automated notifications, and CC-only email are noise — drop them.

## Suggestion rules (for /checkin)
1. Unblock others before advancing my own work.
2. Waiting-on items ≥ 5 days get a nudge suggestion.
3. Sprint issues stale 3+ days, unassigned Highest/High, and releases due
   within 7 days with open issues are all fair game.
4. Cadence items live in `playbook.md`; surface when due or within 2 days.
5. Never suggest something already checked off or snoozed.

## Nothing sends without me
`/draft`, `/weekly`, and nudge suggestions create drafts only. The sole
exceptions are the EOD Slack DM to myself and Jira comments I explicitly
approve one at a time.

## Conversational todo handling
When I talk about my todos in plain language in any session here — "add X",
"remind me Friday to Y", "remove the X one", "done with X", "push X to next
week", "what's on my list" — treat it exactly as a `/todo` request. No slash
command needed. Apply immediately, confirm in one line per change, and only
ask when the match is ambiguous. "Remove" or "drop" deletes the line and
echoes it so I can undo.

## State
`todos.md` is the single source of truth for open work items. Never
restructure it; only add, check off, snooze, or move items per the command
rules. `/morning` updates `last_run`; `/checkin` updates `last_check`; `/todo`
touches neither. `playbook.md` holds cadence items and `last done` dates.
`.helm/scan.md` is a daily cache of the slow Jira sweep (sprint health,
unassigned, support aging) and the P1/P2 board summary, written by `/morning`; `/checkin` reads it instead
of re-querying while it is dated today.
`personal.md` holds non-work items under `## Today / ## Soon / ## Someday /
## Done`; they appear only in the Personal block of briefs, never in drafts,
the weekly update, or meeting prep.

Item line format (all commands use it):
`- [ ] <text> (due YYYY-MM-DD) (snoozed until YYYY-MM-DD) — source: <src> — added YYYY-MM-DD`
Both tags optional. `due` = visible now, escalates to 🔴 when due
today/tomorrow or overdue. `snoozed until` = hidden until that date, then
surfaces as 🔔 now due. A reminder is just a snoozed item.

## Display
Bucket emoji come from `config.md → Display → bucket emoji`, in the order
urgent · needs reply · can wait · FYI. Defaults are 🔴 🟡 🟢 ⚪. Section names
in `todos.md` never change regardless of emoji. `art/` holds optional ASCII
logos printed by `/morning`.

## Tone
Briefs are terse. One line per item with a link. No filler.
