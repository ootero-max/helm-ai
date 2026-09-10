---
description: Draft replies for every "needs reply" item — Gmail/Slack drafts and Jira comments for approval. Never sends.
allowed-tools: Read, Edit, Write, Bash(git add:*), Bash(git commit:*)
argument-hint: [<fragment> | all]
---

You draft my replies. You never send. Input: `$ARGUMENTS`. Read `config.md`
first for my name (sign-off) and boss.

## Scope
- No argument or `all` → every unchecked item in `## Needs reply` of `todos.md`,
  plus any 🔴 item whose action is clearly "reply to X".
- `<fragment>` → the single best-matching open item in any section.
- Never draft for items in `personal.md`.

## Step 1 — Load context for each item
Read `todos.md`. For each item, fetch the source thread so the draft is grounded:
- `source: gmail <id>` → read the thread. Note who wrote last and what they asked.
- `source: slack ...` → read the thread or DM. Note the last message and tone.
- `source: jira KEY` → read the issue and latest comments.
- `source: manual` → draft from the todo text only; say so.
If a tool isn't connected, skip that item with one line saying why.

## Step 2 — Write the draft
One draft per item. Match the channel's register:
- **Email:** greeting, 2–5 sentences, a clear ask or answer, sign-off with my
  first name.
- **Slack:** 1–3 sentences, no greeting, no sign-off.
- **Jira comment:** decision or question first, then reasoning, mention people
  with @ where needed.
Rules: answer what was asked. If a decision is needed and I haven't made it,
draft two variants (yes / not yet) instead of guessing. Never invent dates,
numbers, or commitments not already in `todos.md` or the source thread. Terse
over polished. No "I hope this finds you well."

## Step 3 — Show, then place on approval
Print all drafts first, numbered, each with the item, the channel, and the text.
Then ask in one line: "Place which? (numbers, `all`, or edits)".
On approval:
- Gmail → create a draft reply in the thread (never send).
- Slack → create a message draft in the DM/channel (never send).
- Jira → post the comment (Jira has no drafts; this is the one place a click
  publishes — say so before doing it, and only on explicit yes).
After placing, report one line per item: `Drafted → Gmail: <subject>`.
Do not edit `todos.md`; the item stays open until I confirm I hit send.

## Step 4 — Commit
Nothing to commit unless I asked you to edit a todo; if so
`git add todos.md && git commit -q -m "draft: <summary>" || true`.
