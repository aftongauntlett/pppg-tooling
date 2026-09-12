# AGENTS.md

## What this repo is

Internal tooling for Pretty Pretty Pretty Good (PPPG): client tracking,
inquiry email, and the path to client websites. See
[SPEC.md](./SPEC.md).

This is **not** a client website. New client sites come from
[`aftongauntlett/template`](https://github.com/aftongauntlett/template).
Do not clone this tooling repo to start a job.

Portfolio, resume, and social posting are out of scope.

## Guardrails

- Never commit secrets, API keys, mailbox passwords, or tokens.
- Never silently write, commit, or push to a live client repo. Propose
  a PR (or a diff) unless I explicitly ask to apply it.
- Never auto-send client email. Draft only; I send from Neo. If Neo MCP
  is connected, send-mail tools stay on **needs approval**.
- Do not guess which git repo a client maps to. Use the explicit map or
  ask.
- Slack is connected — do not dump client mail into public channels.
  Linear stays the system of record.
- Ask before running anything that creates ongoing cost (Zapier,
  scheduled cloud agents, paid APIs).
- Keep runs short-leash unless I clear a task for unsupervised work.

## Connections

Linear (PPPG team) and Slack are already authorized in Cursor. Neo MCP
and Zapier/IMAP are not. You cannot complete Neo OAuth or create a
Zapier zap from this session. If mail access is missing, say so.

Do not store the Neo mailbox password in git. An MCP stub in
`.cursor/mcp.json` may list the URL only — never the password.

## Working style

- Solo operator: simple over multi-agent architectures.
- Prefer Neo MCP for reading/drafting mail once it is connected in
  **Cursor** (claude.ai MCP is a separate login and does not power
  this repo).
- Prefer Zapier/Make for “new email → Linear issue” until a scheduled
  MCP poll is proven.
- One agent + CI is enough for static site changes.
- Decisions already in SPEC.md are not open.

## Build order

Follow SPEC.md: mail → Linear first; client sites from
`aftongauntlett/template`. Do not turn this repo into a site starter.
