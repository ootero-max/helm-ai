# Installing Helm AI

Time: about 15 minutes, most of it connecting your accounts. You do this once.

Helm AI runs inside Claude Code and reads your Gmail, Jira, Slack, and
Calendar through your own accounts. Nothing is shared with anyone else. Your
todos and notes live in a private folder on your machine.

---

## 1. Prerequisites

**Claude Code.** Install it if you don't have it:

```
npm install -g @anthropic-ai/claude-code
```

Then run `claude` once in any folder and sign in with your Dazos Claude
account. Docs: https://code.claude.com/docs/en/quickstart

**GitHub access to this repo.** Ask Ozzy to add you as a collaborator on
`ootero-max/helm-ai`. You also need an SSH key on your GitHub account. Test it:

```
ssh -T git@github.com
```

You should see `Hi <your-username>! You've successfully authenticated`. If
not, follow https://docs.github.com/en/authentication/connecting-to-github-with-ssh

**Connect your tools to Claude.** Open https://claude.ai/customize/connectors
and connect:

- Gmail
- Google Calendar
- Slack
- Atlassian (Jira)
- Intercom (optional, only used by meeting prep for customer meetings)

Helm works with whatever is connected and silently skips the rest, so you can
start with Gmail and Jira and add the others later.

---

## 2. Install the plugin

Start Claude Code anywhere and run these two commands inside it:

```
/plugin marketplace add git@github.com:ootero-max/helm-ai.git
/plugin install helm-ai@helm-ai
```

Confirm with:

```
/plugin list
```

You should see `helm-ai@helm-ai` enabled.

---

## 3. Create your workspace

Your workspace is a folder that holds your data. Make one, open Claude Code
inside it, and run setup:

```
mkdir ~/helm
cd ~/helm
claude
/helm-ai:setup
```

Setup asks, one question at a time:

1. Your name, role, company, work email, timezone.
2. Who you report to. Anything from that person is treated as urgent.
3. Your direct reports and the peers you work with most.
4. Your Jira project keys and which one is your support queue.
5. The products or areas you own.
6. Optional extras for the morning brief: a Bible verse, and up to two sports
   teams whose scores appear at the top. Both off by default.
7. A check of which connectors are live, with a fix for any that aren't.
8. Whether you want a 1pm and 5pm summary delivered to your Slack DM.

It then creates your files, initializes git, and writes short aliases so you
can type `/morning` instead of `/helm-ai:morning`.

Re-run any single section later, for example `/setup team` or `/setup extras`.

---

## 4. First run

Tomorrow morning, in your workspace folder:

```
claude
/morning
```

Read the brief, then answer its three questions in plain words. Try adding
something right now:

```
/todo remind me Friday to send the roadmap update
```

---

## 5. Daily commands

| Command | When |
|---|---|
| `/morning` | Start of day. Calendar, everything new, triaged, merged with your list |
| `/checkin` | Midday. Status, what changed, suggested next actions |
| `/checkin 30m` | A free half hour. What to do with it |
| `/checkin eod` | End of day. Done, rolls over, top 3 tomorrow, sent to your Slack DM |
| `/todo ...` | Anytime. Or just say "add X", "remind me Monday to Y", "done with Z" |
| `/draft` | Reply drafts for everything waiting on you. Never sends |
| `/prep next` | Before a meeting |
| `/1on1 <name>` | Before a 1:1. Add `notes: ...` afterwards to record it |
| `/weekly` | Friday. Review plus a draft update to your boss |
| `/helm-ai:helm` | Command list and a health check |

Every command commits your changes to git in your workspace. Nothing is pushed
unless you push.

---

## 6. Back up your workspace (recommended)

Create a **private** repo on GitHub, then in your workspace:

```
git remote add origin git@github.com:<your-username>/helm.git
git push -u origin main
```

Keep it private. Your todos will contain customer names. A private backup also
enables the Slack routines in step 7 and gives you a searchable history:
`git log --oneline -- todos.md` is your work journal.

---

## 7. Optional: Slack summaries at 1pm and 5pm

Two cloud routines can read your workspace and DM you a check-in at 1pm and
a wrap-up at 5pm on weekdays. They are read-only and never edit your files.

Prerequisites, in order:

1. Your workspace is pushed to a private GitHub repo (step 6).
2. GitHub is connected to claude.ai. Open https://claude.ai/code, accept the
   "Connect GitHub" prompt, approve on github.com.
3. The Claude GitHub App is installed on your workspace repo:
   https://github.com/apps/claude, Install, select only that repo.
4. Your connectors from step 1 are live.

Then, in Claude Code inside your workspace, say: "create the routines". Manage
or delete them at https://claude.ai/code/routines.

---

## 8. Updating

Marketplace updates are automatic. To pick up a new version in the current
session:

```
/reload-plugins
```

Your workspace files are never touched by an update.

---

## Troubleshooting

**`Permission denied` or `403` when adding the marketplace.** Your SSH key
isn't tied to a GitHub account with access. Run `ssh -T git@github.com` and
check the username it greets. Ask Ozzy to add that account as a collaborator.

**`/helm-ai:setup` says unknown command.** The plugin loaded after your session
started. Run `/reload-plugins` or start a new `claude` session.

**"No config.md here — run /setup first."** You're in the wrong folder. `cd`
into your workspace before starting Claude Code.

**A connector shows as not connected during setup.** Connect it at
https://claude.ai/customize/connectors, then run `/setup connectors` again.

**The brief misranks something.** Don't correct it verbally each time. Encode
the rule: urgency and suggestion rules live in your workspace `CLAUDE.md`,
standing responsibilities in `playbook.md`. Send command improvements back to
this repo as a pull request so everyone benefits.

**Something else.** Ask Ozzy, or open an issue on this repo.
