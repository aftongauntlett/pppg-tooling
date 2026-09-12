# AGENTS.md

## What this repo is

Internal studio playbook for Pretty Pretty Pretty Good (PPPG): thin
client index, Linear project-per-client, drafted replies. See
[SPEC.md](./SPEC.md).

This is **not** a client website. New client sites come from
[`aftongauntlett/template`](https://github.com/aftongauntlett/template).
Do not clone this tooling repo to start a job.

Portfolio, resume, social posting, and mailbox automation are out of
scope.

## Guardrails

- Never commit secrets, API keys, or tokens.
- Never silently write, commit, or push to a live client repo. Propose
  a PR (or a diff) unless I explicitly ask to apply it.
- Never send client email. Draft a reply; I send it.
- Do not guess which git repo a client maps to. Use
  [clients.md](./clients.md) or ask. If there is no repo yet, say so
  on the Linear project and in the index.
- Do not copy payments or email transcripts into git. Those live on
  the Linear project.
- Linear is the system of record. Slack is optional intake, not the
  tracker. Do not dump client mail into public Slack channels.
- Ask before running anything that creates ongoing cost (scheduled
  cloud agents, paid APIs).
- Keep runs short-leash unless I clear a task for unsupervised work.

## Intake (do this when I name a new client or paste an inquiry)

Required from me: **client name**, **email**, **git repo** (or “none
yet”). If any of those are missing, ask — do not invent them.

Then, in one pass:

1. Add or update the row in [clients.md](./clients.md) (name, repo,
   email, Linear project URL once you have it).
2. Create a Linear **project** named after the client. Summary: email,
   git repo (or “no repo yet”), rate if known. Description: short
   brief. File `Intake: {Client name}` in that project with what they
   want.
3. If Customer Requests is enabled, create or reuse a Linear Customer
   (by name if the email is gmail/icloud) and link it. Set revenue /
   tier only when I have given those numbers.
4. Draft a reply I can copy-send. Put the draft in chat. After I send
   it, paste a short summary into the Intake issue or the project Log
   if I share the sent mail.

Later work: new issue on **that same project**. Completed date = when
the work was done. When I report a payment, add a dated line to the
project description and update Customer revenue if it exists.

Do not create a GitHub repo unless I ask. Do not touch the template
repo unless I ask to change the starter itself.

## Connections

Linear (PPPG team) and Slack are already authorized in Cursor. You
cannot enable Customer Requests or create workflow states from here;
if a project or Customer cannot be created, say so and use what
exists.

## Working style

- Solo operator: simple over multi-agent architectures.
- One agent + CI on the client site is enough for static site changes.
- Decisions already in SPEC.md are not open.

## Build order

Follow SPEC.md: Cursor chat → Linear project + clients.md first.
Client sites from `aftongauntlett/template`. Do not turn this repo
into a site starter. Do not add Slack intake unless I ask.
