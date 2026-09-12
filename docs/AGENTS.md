# AGENTS.md

## What this repo is

Internal automation tooling for Pretty Pretty Pretty Good (PPPG), a solo
pro-bono/client web studio. See [SPEC.md](./SPEC.md) for the full spec.

This is **not** a client website. Do not clone it to start a client job.
Client sites come from `pppg-template` (GitHub template) once that repo
exists.

## Guardrails

- Never commit secrets, API keys, mailbox passwords, or tokens. Use
  environment variables / Cursor secrets. Do not invent a committed
  `.env` with real values.
- Never silently write, commit, or push to portfolio, resume, or live
  client repos. Propose a PR (or a diff) and stop unless I explicitly
  ask to apply it.
- Never auto-send client email. Draft only; I send from Neo.
- Never auto-merge to `main`. The PR is the approval gate.
- Social posting is out of scope. Do not draft or publish social posts.
- Ask before running anything that creates ongoing cost (Zapier tasks,
  scheduled cloud agents, paid APIs). Flag estimated cost first.
- Keep runs short-leash by default. Do not assume long unsupervised
  execution unless I say this task is cleared for it.

## Connections

You cannot OAuth Linear, enable Neo IMAP, or create Zapier zaps from
this session. I have to click those. Do not pretend they are connected.
If a task needs Linear or mail and the integration is missing, say so
and point at the Connections section in SPEC.md.

Do not store the Neo mailbox password in git.

## Resume / Desktop

`aftongauntlett/resume` builds `afton-gauntlett-resume.pdf`. On **my
machine**, that file should replace `~/Desktop/afton-gauntlett-resume.pdf`.
A cloud agent’s `~/Desktop` is the wrong machine — update the resume
repo via PR instead, and leave the Desktop copy to a local build.

Stay at 2 pages (`pdfinfo`). If content will not fit, condense older
entries and tell me what you traded off.

## Working style

- Solo operator: simple, maintainable solutions over multi-agent
  architectures.
- Prefer Zapier/Make for the first version of email → Linear.
- One agent + CI on a PR is enough for static site changes. Do not
  spawn extra review/test agents unless I ask.
- When a design decision is still open, lay out tradeoffs and ask
  rather than picking silently. Decisions already recorded in SPEC.md
  are not open.
- Explain what you built well enough that I can maintain it later.
  Brief comments only on non-obvious bits.

## Build order

Follow SPEC.md: Part 1 (portfolio → resume) → Part 3 (email → Linear
issue) → Part 2 (`pppg-template`). Template implementation happens in
that other repo, not by turning this tooling repo into a site starter.
