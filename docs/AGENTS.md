# AGENTS.md

## What this repo is

Internal tooling for Pretty Pretty Pretty Good (PPPG): client tracking,
inquiry email, and the path to client websites. See
[SPEC.md](./SPEC.md).

This is **not** a client website. Do not clone it to start a client job.
Client sites come from `pppg-template` once that repo exists.

Portfolio, resume, and social posting are out of scope. Do not touch
those repos from this tooling.

## Guardrails

- Never commit secrets, API keys, mailbox passwords, or tokens.
- Never silently write, commit, or push to a live client repo. Propose
  a PR (or a diff) unless I explicitly ask to apply it.
- Never auto-send client email. Draft only; I send from Neo. If Neo MCP
  is connected, send-mail tools stay on **needs approval**.
- Never auto-merge to `main`. The PR is the approval gate.
- Do not guess which git repo a client maps to. Use the explicit map or
  ask.
- Ask before running anything that creates ongoing cost (Zapier,
  scheduled cloud agents, paid APIs).
- Keep runs short-leash unless I clear a task for unsupervised work.

## Connections

You cannot OAuth Linear, add Neo MCP, or create Zapier zaps from this
session. I have to click those. If mail or Linear is missing, say so
and point at SPEC.md Connections. Do not store the Neo mailbox
password in git.

## Working style

- Solo operator: simple over multi-agent architectures.
- Prefer Neo MCP for reading/drafting mail once it is connected in
  Cursor. Prefer Zapier/Make for “new email → Linear issue” until a
  scheduled MCP poll is proven.
- One agent + CI on a PR is enough for static site changes.
- Decisions already in SPEC.md are not open. Ask only when something
  is still undecided.
- Explain what you built well enough that I can maintain it later.

## Build order

Follow SPEC.md: Part 1 (email + Linear) → Part 2 (`pppg-template` in
its own repo). Do not turn this tooling repo into a site starter.
