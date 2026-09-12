# pppg-tooling

Internal studio playbook for [Pretty Pretty Pretty Good](https://www.prettyprettyprettygood.org/):
which clients exist, which git repo is theirs, and how work shows up in Linear.

This repo is **not** a client website. New sites are created from the
public GitHub template
[`aftongauntlett/template`](https://github.com/aftongauntlett/template)
(Astro, WCAG 2.2, `PROJECT_BRIEF.md` kickoff).

## What's here

- [docs/SPEC.md](docs/SPEC.md) — how this repo, Linear, Slack, and the
  template fit together
- [docs/AGENTS.md](docs/AGENTS.md) — rules for agents working here
- [docs/clients.md](docs/clients.md) — thin index: name, repo, email,
  Linear project URL

## How intake works

Open this repo in Cursor and say you have a new client (or paste the
inquiry). Give **client name**, **git repo** (or say it does not exist
yet), and **email**.

The agent then:

1. Adds or updates the row in `docs/clients.md`
2. Creates a Linear **project** for that client (dossier + first
   Intake issue)
3. Drafts a reply you can send yourself

You send the email. Linear is the client file and the tracker. Site
work happens in the client repo, not here.

Slack is connected and can do the same intake later if you want it from
your phone. It is not required. Do not treat Slack as the system of
record.

## Current setup

| Piece | Status |
|---|---|
| Linear ↔ Cursor (PPPG team) | Connected |
| Slack ↔ Cursor | Connected |
| Client site template | [`aftongauntlett/template`](https://github.com/aftongauntlett/template) |
| Client → repo map | [docs/clients.md](docs/clients.md) |

Portfolio, resume, and social posting are out of scope.

## Start a client site

1. On GitHub: **Use this template** on
   [`aftongauntlett/template`](https://github.com/aftongauntlett/template)
   (or `gh repo create --template aftongauntlett/template`).
2. Fill `PROJECT_BRIEF.md`.
3. Run `npm install`, then `npm run detach-template`.
4. Open the new repo in Cursor and use the new-site-kickoff prompt.
5. Add the repo to [docs/clients.md](docs/clients.md) and the Linear
   project summary.

Do not clone `pppg-tooling` for that.

## Setup (this repo)

No app secrets live here. Linear and Slack use Cursor’s integrations.
