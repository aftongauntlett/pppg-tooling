# pppg-tooling

Internal tooling for Pretty Pretty Pretty Good (PPPG): client tracking,
inquiry email, and the path to new client websites.

This repo is **not** a client website starter. New client sites will
come from `pppg-template` (a GitHub template, created separately).

## What's here

- [docs/SPEC.md](docs/SPEC.md) — spec: Neo mail → Linear, client
  website template plan, and which connections I have to make in the
  browser (Linear, Neo MCP, optional Zapier).
- [docs/AGENTS.md](docs/AGENTS.md) — rules for any agent working here.

## Status

Docs only. Portfolio/resume sync and social posting are out of scope.
Linear, Neo MCP, and any IMAP zap are still **manual** (agents cannot
complete OAuth).

## Related repos

- Studio site:
  [`prettyprettyprettygood`](https://github.com/aftongauntlett/prettyprettyprettygood)
- Production references for the future template:
  [`rcan`](https://github.com/aftongauntlett/rcan),
  [`astrid-beauty`](https://github.com/aftongauntlett/astrid-beauty)
- Existing SvelteKit starter (**not** the client template):
  [`sveltekit-starter`](https://github.com/aftongauntlett/sveltekit-starter)

## Setup

No app secrets in this repo yet. When automations need keys, they go in
Cursor secrets or a gitignored `.env` — never committed.
