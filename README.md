# pppg-tooling

Internal tooling for [Pretty Pretty Pretty Good](https://www.prettyprettyprettygood.org/):
client tracking, inquiry email, and the path to new client websites.

This repo is **not** a client website. New sites are created from the
public GitHub template
[`aftongauntlett/template`](https://github.com/aftongauntlett/template)
(Astro, WCAG 2.2, `PROJECT_BRIEF.md` kickoff).

## What's here

- [docs/SPEC.md](docs/SPEC.md) — how mail, Linear, Slack, and the
  template fit together
- [docs/AGENTS.md](docs/AGENTS.md) — rules for agents working here

## Current setup

| Piece | Status |
|---|---|
| Linear ↔ Cursor (PPPG team) | Connected |
| Slack ↔ Cursor | Connected |
| Client site template | [`aftongauntlett/template`](https://github.com/aftongauntlett/template) (public GitHub template) |
| Neo MCP in Cursor | Not connected yet (OAuth in the browser) |
| New mail → Linear issue | Not wired yet (Zapier/Make IMAP, or a scheduled agent once Neo MCP works) |

Portfolio, resume, and social posting are out of scope.

## Start a client site

1. On GitHub: **Use this template** on
   [`aftongauntlett/template`](https://github.com/aftongauntlett/template)
   (or `gh repo create --template aftongauntlett/template`).
2. Fill `PROJECT_BRIEF.md`.
3. Run `npm install`, then `npm run detach-template`.
4. Open the new repo in Cursor and use the new-site-kickoff prompt.

Do not clone `pppg-tooling` for that.

## Setup (this repo)

No app secrets live here. Linear and Slack use Cursor’s integrations.
If we add Neo MCP or Zapier later, credentials stay in Cursor / Zapier,
never in git.
