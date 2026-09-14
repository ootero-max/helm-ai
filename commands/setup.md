---
description: First-run configuration for Helm AI — who you are, your boss, team, Jira, connectors, extras — or redo one section
allowed-tools: Read, Edit, Write, Bash(git init:*), Bash(git add:*), Bash(git commit:*), Bash(git config:*), Bash(git status:*), Bash(mkdir:*), Bash(ls:*), Bash(cp:*), Bash(date:*)
argument-hint: [me | boss | team | jira | products | extras | connectors | routines | aliases]
---

You are setting up Helm AI for a new user, or redoing one section. Input:
`$ARGUMENTS`. Plugin files are at `${CLAUDE_PLUGIN_ROOT}`. The workspace is
the current directory. Be warm but brief: one question at a time, accept
short answers, never ask for anything you can infer.

## If the workspace is empty (first run)
Say in two lines what Helm AI is and that this takes about three minutes.
Then copy templates that don't already exist (never overwrite existing files):
- `${CLAUDE_PLUGIN_ROOT}/templates/config.md` → `config.md`
- `${CLAUDE_PLUGIN_ROOT}/templates/CLAUDE.md` → `CLAUDE.md`
- `${CLAUDE_PLUGIN_ROOT}/templates/todos.md` → `todos.md`
- `${CLAUDE_PLUGIN_ROOT}/templates/personal.md` → `personal.md`
- `${CLAUDE_PLUGIN_ROOT}/templates/playbook.md` → `playbook.md`
- `${CLAUDE_PLUGIN_ROOT}/templates/routines/README.md` → `routines/README.md`
- create `1on1/` with a one-line README.
- `.gitignore` with `.DS_Store`, `.claude/settings.local.json`, `.claude/scheduled_tasks.lock`.
Then run every section below in order. If an argument names a section, run
only that one and skip to "Finish".

## Sections (fill the matching block in `config.md`; one line per value)

**me** — Ask, in one message: full name, role/title, company, work email,
timezone (offer the machine's timezone as default via `date +%Z`). Set the
git identity for this workspace from name + email.

**boss** — "Who do you report to, and their title?" Explain in one clause
that anything from this person is treated as urgent.

**team** — "Who are your direct reports? And who are the peers you work with
most?" First names as they appear in Slack. Comma-separated. Either may be
empty.

**jira** — "Which Jira project keys matter to you day to day? And which one
is your support/ticket queue, if any?" Uppercase keys. If they named a
support project: "Does it have a status for tickets escalated to engineering?
Exact name, or skip." → `escalated status`.

**products** — "What products or areas do you own? One line each." Used to
group weekly and support summaries.

**extras** — Two optional touches for the top of the morning brief:
1. "Want a short Bible verse at the top of your morning brief? (yes/no) If
   yes, which translation? Default ESV." Set `verse: on|off` and translation.
2. "Want a line with your favorite sports teams' latest result and next game?
   Up to two teams, any league." Set `sports: on|off` and the `teams` list as
   `emoji Team Name (League)`. Pick a fitting emoji yourself.
3. If they named teams: "Want an ASCII logo at the top of the brief? It
   alternates between your teams." If yes, set `logo: on` and draw one piece
   of ASCII art per team into `art/<team-slug>.txt` (slug = full team name,
   lowercase, letters only, league dropped: `miamidolphins`, `intermiamicf`). Max 8 lines × 60 columns, plain ASCII plus block
   characters, the team name and one short motto on the right. Show each one
   in a code block and offer one redraw.
4. If they named teams: "Want the urgency markers in your teams' colors
   instead of the classic 🔴 🟡 🟢 ⚪?" If yes, set `Display → bucket colors:
   team` and choose four emoji from the teams' colors, hottest color for
   urgent, e.g. 🟠 🩵 🟢 ⚪. Otherwise leave classic.
All default to off. Never push.

**connectors** — Detect what's connected by attempting one cheap read with
each tool family: Gmail (search 1 message), Jira (search 1 issue), Slack (read
own profile), Google Calendar (list calendars). Report a four-line checklist:
✅ connected / ❌ not connected. For each ❌, give the one-line fix:
- claude.ai connectors: https://claude.ai/customize/connectors (Gmail,
  Google Calendar, Slack, Atlassian, Intercom).
- Or inside Claude Code: `/mcp` to authorize a configured server.
Say that Helm works with whatever is connected and skips the rest silently.
Never ask for passwords, tokens, or codes.

**routines** — "Want a 1pm check-in and a 5pm end-of-day summary delivered to
your Slack DM on weekdays? They run in the cloud and are read-only." If yes:
set `slack briefs: on`, compute the UTC cron from their timezone (1pm and 5pm
local, Mon–Fri), and write `routines/checkin-1pm.json` and
`routines/eod-5pm.json` following the shape in
`${CLAUDE_PLUGIN_ROOT}/templates/routines/README.md`'s prerequisites: the
prompt text should mirror `/checkin` full mode and eod mode, unattended,
read-only, DM to the user's own Slack by their email, model `claude-sonnet-5`.
Then tell them the three prerequisites from that README. Do not try to create
the routines during setup.

**aliases** — So the user can type `/morning` instead of `/helm-ai:morning`:
for each of morning, checkin, todo, draft, prep, weekly, 1on1, setup, write
`.claude/commands/<name>.md` from `${CLAUDE_PLUGIN_ROOT}/templates/alias.md`,
replacing `{{COMMAND}}`, `{{DESCRIPTION}}` (copy from the plugin command's
frontmatter), and `{{ARGUMENT_HINT}}` (same; empty if none). Skip files that
already exist. Tell the user first, in one line, that Claude Code will ask
them to approve writing these files because they live under `.claude/`; they
should answer yes. If a write is denied, list the aliases that were skipped
and say `/setup aliases` can finish it later.

## Finish
- If not already a git repo: `git init -q`, then
  `git add -A && git commit -q -m "helm-ai: initial setup"`.
  Otherwise commit only the files you changed with message
  `helm-ai: setup <sections>`.
- Print a five-line summary: who/where, what's connected, extras chosen, and
  the two things to run next: `/morning` tomorrow morning, `/todo <anything>`
  right now. Mention that pushing this workspace to a **private** GitHub repo
  gives them history and enables the routines.
- If any connector was ❌, repeat its fix line at the end.
