# pppg-tooling — Studio spec

## What this repo is for

Pretty Pretty Pretty Good (PPPG) is a solo pro-bono/client web studio.
This repo is the **studio playbook**, not a product and not a website
starter.

Use it to:

- Remember which clients exist and which git repo is theirs
- Turn a new inquiry into a Linear **project** (the client file) plus
  an Intake issue
- Draft a reply (you send the email yourself)
- Point agents at the right client repo later

Never clone this repo to start a client job. Client sites come from
[`aftongauntlett/template`](https://github.com/aftongauntlett/template)
(“Use this template”). Never silently write to a live client repo —
propose a PR unless I explicitly ask to apply it.

Personal portfolio, resume, and social posting are **out of scope**.
Mailbox automation (Neo MCP, Zapier, IMAP, auto-reply) is **out of
scope**. You handle email. Do not put payment history or email
transcripts in git.

### Related repos — keep them loosely coupled

| Repo | Role |
|---|---|
| `aftongauntlett/pppg-tooling` (this repo) | Playbook, thin client index, intake rules |
| [`aftongauntlett/template`](https://github.com/aftongauntlett/template) | Public GitHub template for new client sites |
| `aftongauntlett/prettyprettyprettygood` | Studio marketing site (not the client starter) |
| Live client repos (`rcan`, `astrid-beauty`, …) | Production sites; do not fork these as the template |

**Do not git-connect this repo to the template** (no submodule, no
monorepo, no shared CI). The only links are:

- This spec saying new sites come from the template
- [clients.md](./clients.md) listing each live `owner/repo` and Linear
  project URL

Template improvements happen in the template repo, and only when a
new site is missing something in the starter. Studio process changes
happen here. A client site never needs this repo on disk.

---

## Operating model

Three surfaces, one job each:

| Surface | Job |
|---|---|
| **This repo in Cursor** | Primary operator UI. “New client: Name, repo, email.” |
| **Linear** | Client file and work tracker. One project per client. |
| **Client git repo** | Where the website actually gets built. |

Slack is optional. Email is manual. [clients.md](./clients.md) is a
thin index for agents (name, repo, email, Linear project URL) — not
the dossier.

### Why Cursor chat is the default

You are a solo operator already in Cursor when real work happens. A
Slack bot that creates a Linear ticket still leaves you opening the
client repo to build the site. Putting intake in this chat means the
same session can update [clients.md](./clients.md), create the Linear
project, and draft the reply — with the playbook already in context.

Slack **is** powerful enough for the narrow job “paste name / repo /
email → Linear project + drafted reply.” It is **not** powerful enough
to be the studio OS. Unstructured Slack messages still need the client
map and these rules, or the agent guesses.

Use Slack later only if you want intake from your phone. Then a Cursor
Automation on a **private** channel can follow this spec. Until that
hurts, skip it.

### Linear — the client file

Cursor ↔ Linear is authorized for the PPPG team.

Linear does not ingest mailboxes and has no arbitrary custom fields.
History exists when you (or an agent) write it down. Paste emails as
you go. That is enough.

| Place | What it holds |
|---|---|
| **One project per client** | The page you open. Summary: email, git repo, rate. Description: living brief plus a short dated money list (`YYYY-MM-DD · $amount · what for`). |
| **Issues in that project** | Work items. Created/completed dates are the work history. First issue is `Intake: {Client name}`. |
| **Comments, or a project doc named Log** | Emails and conversation notes you paste. Timestamped, searchable. |
| **Linear Customer** (optional) | Name, status, tier (paid vs pro-bono), **revenue** as lifetime paid. Enable Customer Requests, create manually, link to the project. |

Personal gmail/icloud addresses do not map to a Customer by domain —
create those by name.

Do not guess the repo. If it is missing, ask or write “no repo yet”
on the project summary **and** in [clients.md](./clients.md).

Issue states: Intake → In Progress → Done is enough. I approve
merges. I send mail. Nothing auto-merges or auto-sends.

### Slack (optional, later)

Cursor ↔ Slack is already integrated.

If we add it:

- Private channel only (e.g. `#pppg-intake`)
- Message shape: client name, git repo, email, plus any notes
- Agent replies in the thread with the Linear **project** link and a
  drafted email
- Linear stays the system of record; Slack is a trigger
- Do not dump full client mail into public channels

The automation’s instructions should say “follow `docs/AGENTS.md` and
`docs/clients.md` in pppg-tooling.” Do not duplicate a second spec
inside Slack.

### Client map

[clients.md](./clients.md) is the explicit `client → git repo → email
→ Linear project` index. Agents must not invent a repo. New client or
a repo change → update that file in the same pass as the Linear
project. Do not copy payments or email transcripts into git.

---

## Intake flow (v1)

1. You open this repo (or later, Slack) and pass **name**, **repo**,
   **email**, and what they want. Optional: what they paid, or
   pro-bono.
2. Agent updates [clients.md](./clients.md).
3. Agent creates a Linear **project** named after the client, fills
   the summary, and files `Intake: {Client name}` inside it. If a
   Customer record is in play, create or update it and set revenue /
   tier when you know them.
4. Agent drafts a reply. You send it. Paste the sent-mail summary into
   the Intake issue or the project Log.
5. If there is no site yet: create one from the template, then add the
   new `owner/repo` to the map and the project summary.
6. Site work happens **in that client repo**. Prefer a draft PR for
   unattended work. Local edits on `main` are fine when you are at the
   keyboard. Each job is an issue on that Linear project.
7. Update Linear as work moves. Completed issue date = when the work
   was done. Money line goes on the project description when paid.

One agent + CI on the client site is enough. No extra review/test
agents unless I ask.

**Not v1:** mailbox polling, auto-send, Slack intake automation,
unattended repo selection, three-agent chains, template changes unless
a new site is missing something in the starter.

---

## Client websites (`aftongauntlett/template`)

**Use the existing public template.** Do not create a second starter
and do not clone this tooling repo to begin a job.

[`aftongauntlett/template`](https://github.com/aftongauntlett/template)
is an Astro 7 GitHub template aimed at PPPG work:

- “Use this template” → new client repo with unrelated history
- `PROJECT_BRIEF.md` as the kickoff input
- WCAG 2.2 AA in docs + eslint `jsx-a11y`
- `npm run detach-template` strips maintainer-only docs on new sites
- `npm run validate` and pre-commit hooks for solo `main`

Each client: GitHub **Use this template** (or
`gh repo create --template aftongauntlett/template`), fill
`PROJECT_BRIEF.md`, run the new-site-kickoff prompt, then record the
repo here and on the Linear project.

Do not put client CRM (payments, emails, Linear) in the template.
Keep evolving that template rather than starting over, when a new
site actually needs a starter change.

Lock to **Astro**. A database app / mobile app / game is a different
starter later.

---

## Connections

| Integration | Status |
|---|---|
| Linear ↔ Cursor (PPPG team) | **Done** |
| Slack ↔ Cursor | **Done** (intake automation not set up) |
| GitHub | Connected for this repo; client repos as listed in clients.md |
| Mailbox / Zapier / Neo MCP | **Out of scope** |

Agents can use Linear (and Slack, if asked) once a session has those
tools. They cannot enable Customer Requests or click new workflow
states into existence; if a project or Customer record cannot be
created, say so and use what exists.

---

## Build order

1. Keep [clients.md](./clients.md) current. Intake via this Cursor
   chat → Linear project + Intake issue + drafted reply.
2. Client sites live in repos created from `aftongauntlett/template`.
3. Slack intake automation only if phone-side intake becomes a real
   habit.

Do not implement mailbox sync, SMTP auto-send, portfolio/resume sync,
social posting, or a second website template.
