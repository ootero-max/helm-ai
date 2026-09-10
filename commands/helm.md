---
description: What Helm AI can do — the command list and a health check of this workspace
allowed-tools: Read
---

Print a compact help card for Helm AI, then a one-line health check.

**Commands** (one line each, with the alias if `.claude/commands/` exists here):
- `/morning` — full triage before the day starts
- `/checkin` · `/checkin 30m` · `/checkin eod` — daytime status, time-boxed picks, wrap-up
- `/todo <text>` · `due <when>` · `remind <when>: <text>` · `done <x>` · `list` — or just talk to me
- `/draft [item]` — reply drafts for the 🟡 bucket, never sends
- `/prep [next|tomorrow|<meeting>]` — one-screen meeting brief
- `/1on1 <name> [notes: ...]` — prep and record a 1:1
- `/weekly` — review the week, draft the update to my boss
- `/setup [section]` — configure or reconfigure

**Health:** check that `config.md`, `todos.md`, `personal.md`, `playbook.md`
exist here and that `config.md → Me → name` is filled. Report ✅ or the one
thing to fix. Don't call any external tool.
