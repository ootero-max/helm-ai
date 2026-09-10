# Scheduled routines (cloud agents)

Two read-only cloud routines post briefs to your Slack DM on weekdays. They
read this workspace repo for context but never edit it, so they can't conflict
with your local commits.

| Routine | Local | What it sends |
|---|---|---|
| 1pm check-in | 1:00pm | Status, delta, suggestions, personal |
| 5pm end of day | 5:00pm | Done today, rolls over, top 3, tomorrow's calendar |

Cron runs in UTC. `/setup routines` converts your local times and writes the
two JSON files here.

## Prerequisites
1. This workspace is pushed to a **private** GitHub repo.
2. GitHub is connected to claude.ai: open https://claude.ai/code, accept the
   "Connect GitHub" prompt, approve on github.com.
3. The Claude GitHub App is installed on the workspace repo:
   https://github.com/apps/claude → Install → select only this repo.
4. Gmail, Atlassian, Google Calendar, and Slack are connected as claude.ai
   connectors at https://claude.ai/customize/connectors.

Then say "create the routines" in a Claude Code session here. Manage or delete
them at https://claude.ai/code/routines.
