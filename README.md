# pppg-tooling

Internal automation tooling for Pretty Pretty Pretty Good (PPPG) — a solo
pro-bono/client web studio.

This repo is **not** a client website starter. New client sites will
come from `pppg-template` (a GitHub template, created separately).

## What's here

- [docs/SPEC.md](docs/SPEC.md) — spec: portfolio → resume sync, the
  client Astro template plan, and Neo email → Linear → PR pipeline,
  plus which connections I have to make in the browser.
- [docs/AGENTS.md](docs/AGENTS.md) — rules for any agent working here.

## Status

Docs only. Social posting was dropped. Linear and Neo/Zapier connections
are still **manual** (OAuth / IMAP zaps cannot be completed from a
cloud agent).

## Related repos

- [`react-portfolio`](https://github.com/aftongauntlett/react-portfolio) —
  job history source for resume sync
- [`resume`](https://github.com/aftongauntlett/resume) — JSON → 2-page PDF;
  Desktop copy happens on a local build
- Production references for the future template:
  [`rcan`](https://github.com/aftongauntlett/rcan),
  [`astrid-beauty`](https://github.com/aftongauntlett/astrid-beauty)
- Existing SvelteKit starter (**not** the client template):
  [`sveltekit-starter`](https://github.com/aftongauntlett/sveltekit-starter)

## Setup

No app secrets in this repo yet. When automations need keys (Linear,
Zapier, etc.), they go in Cursor secrets or a gitignored `.env` — never
committed.
