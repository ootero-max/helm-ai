# Playbook — standing responsibilities

`/checkin` reads this file and surfaces anything whose `last done` + cadence is
past or within 2 days. Edit freely: add rows, change cadences, fix dates.
When you tell `/checkin` you finished one, it updates `last done`.

Date format: YYYY-MM-DD. Leave `last done` blank to have it surface immediately.

## Cadence

| Item | Cadence | last done | Notes |
|---|---|---|---|
| Roadmap review across my products | weekly | | Are we still building the right things this sprint? |
| 1:1s with each direct report | weekly | | Team list in config.md; Slack recency is the proxy signal |
| Sprint review / demo attendance | per sprint | | |
| Product metrics review | monthly | | |
| Support trend review: top recurring issues by product | biweekly | | Pattern fixes > ticket fixes |
| Release notes / customer comms review before each release | per release | | Check with CS before it ships |
| Customer conversations (at least 2) | biweekly | | Sales, CS, or direct |
| Competitive / market scan | monthly | | |
| Goals / OKR check-in with my boss | quarterly | | |
| Backlog grooming with leads | biweekly | | Unassigned Highest/High should be zero |
| Design review sync | weekly | | |
| Security / access hygiene (2FA, API keys, repos) | quarterly | | |

## Suggestion heuristics (how /checkin ranks)

1. Unblock others before advancing your own work.
2. Anything touching your boss or a customer escalation outranks internal work.
3. A nudge that costs 2 minutes and unblocks a 5-day wait is always worth it.
4. Prefer pattern-level fixes (a cluster of support tickets) over single tickets.
5. Fit the top suggestion to the next free calendar block.
