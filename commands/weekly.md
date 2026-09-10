---
description: Weekly review — what shipped, what slipped, support trend, missed cadences, and a draft update to my boss
allowed-tools: Read, Edit, Write, Bash(git log:*), Bash(git add:*), Bash(git commit:*)
argument-hint: [week | last week]
---

You run my weekly review. Input: `$ARGUMENTS`. Default window: Monday of the
current week through today. `last week` → the previous Mon–Sun.
Read `config.md` first (boss, team, Jira projects, support project, products).

## Step 1 — Load state
Read `todos.md`, `playbook.md`, `CLAUDE.md`. Run
`git log --since=<window start> --format='%h %ad %s' --date=short -- todos.md`
to see the week's changes, and `git log -p --since=<window start> -- todos.md`
to recover every item that entered `## Done` or `## Waiting on` this week.

## Step 2 — Gather (parallel; skip silently if a tool is missing)
- **Jira, shipped:** issues resolved in the window across my projects — count
  per project, list Highest/High and anything customer-visible.
- **Jira, slipped:** issues in a sprint that ended this week and were not
  done; fixVersions whose release date passed with open issues.
- **Support trend:** created vs resolved this week in the support project;
  still untouched > 2 biz days; top 3 recurring themes by component or product.
- **Calendar:** count of meetings and hours; external meetings by company.
- **Slack recency:** for each direct report, days since last DM exchange.

## Step 3 — Review (for me)
Terse, one line per item with link. Sections:
1. **Shipped** — by product; the 3–5 items that matter most first.
2. **Slipped / at risk** — what, why if known, owner.
3. **Done from my list** — items checked off this week (from git).
4. **Still open ≥ 7 days** — my todos that have been sitting; propose drop/delegate.
5. **Support** — created/resolved, aging count, top themes, one pattern-fix suggestion.
6. **Cadence** — playbook items missed this week; items due next week.
7. **People** — anyone on the Team list I haven't talked to in 7+ days.
8. **Next week's top 3** — proposed, from the 🔴 bucket and release dates.

## Step 4 — Draft update to my boss
Write the update as a Slack message draft to my boss (or a Gmail draft if
Slack is missing). Format: 5–8 bullets max. Shipped → risks → asks. Plain
words, no Jira jargon, no ticket keys unless they'd recognize them. Numbers
only where they change a decision. Do NOT send. Show it to me first; on
approval create the draft. Never include anything from `personal.md`.

## Step 5 — Update loop and commit
Ask: "Anything to drop, delegate, or add before next week?" Apply the same
`todos.md` rules as `/checkin`. Update `playbook.md` `last done` for any
cadence item I confirm. Then:
`git add todos.md playbook.md && git commit -q -m "weekly: $(date +%F)" || true`
