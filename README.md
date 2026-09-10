# Helm AI

A daily command center for product leaders, running inside Claude Code. It
pulls Gmail, Jira, Slack, and Calendar, triages by urgency, keeps your todo
list as a plain markdown file with git history, and suggests what to do next.

Built at Dazos for the product team. Private.

## What you get

| Command | When | What it does |
|---|---|---|
| `/morning` | Before coffee | Calendar, then everything new, triaged 🔴🟡🟢⚪, merged with your open list |
| `/checkin` | Midday | Status, what changed, 3–5 suggested next actions |
| `/checkin 30m` | A free half hour | "What should I do with 30 minutes?" |
| `/checkin eod` | End of day | Done today, rolls to tomorrow, top 3, sent to your Slack DM |
| `/todo ...` | Anytime | Add, due dates, reminders, done, snooze, drop. Or just say it in plain words |
| `/draft` | When replies pile up | Gmail/Slack drafts for every "needs reply" item. Never sends |
| `/prep next` | Before a meeting | Attendees, last exchanges, their Jira, what you owe each other |
| `/1on1 <name>` | Before a 1:1 | Their load, wins, talking points; records notes after |
| `/weekly` | Friday | Shipped, slipped, support trend, cadence misses, draft update to your boss |

Optional: a 1pm check-in and 5pm wrap delivered to your Slack DM by a cloud
routine, and a morning opener with a Bible verse and your teams' scores.

## Install (once)

Prerequisite: Claude Code and SSH access to this repo.

```
/plugin marketplace add git@github.com:ootero-max/helm-ai.git
/plugin install helm-ai@helm-ai
```

Then make a private folder for your data and run setup:

```
mkdir ~/helm && cd ~/helm && claude
/helm-ai:setup
```

Setup takes about three minutes: who you are, who you report to, your team,
your Jira projects, optional extras, and a check of which connectors are
live. It also creates short aliases so `/morning` works instead of
`/helm-ai:morning`.

Connect Gmail, Google Calendar, Slack, and Atlassian at
https://claude.ai/customize/connectors. Helm works with whatever is connected
and skips the rest.

## Your data stays yours

Everything personal lives in your workspace folder: `config.md`, `todos.md`,
`personal.md`, `playbook.md`, `1on1/`. This plugin repo contains no user data.
Every command commits your changes locally; push the workspace to a **private**
GitHub repo for history and backup.

## Updating

Marketplace updates are automatic. Run `/reload-plugins` to pick up a new
version in the current session.

## Layout

```
.claude-plugin/plugin.json      manifest
.claude-plugin/marketplace.json this repo is its own marketplace
commands/*.md                   the nine commands
templates/                      files /setup copies into a new workspace
```

## Tuning

When a brief misranks something, encode the rule instead of correcting it
verbally: urgency and suggestion rules live in your workspace `CLAUDE.md`,
cadence items in `playbook.md`, and sources/formats in the command files here.
Send command improvements back as a PR so everyone gets them.
