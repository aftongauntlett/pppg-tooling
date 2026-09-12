# pppg-tooling — Automation Spec

## Context

Pretty Pretty Pretty Good (PPPG) is a solo pro-bono/client web studio.
This repo is **internal studio tooling only**: client tracking, inquiry
email, and the path from a request to a website.

Never clone this repo to start a client job. Client sites come from
[`aftongauntlett/template`](https://github.com/aftongauntlett/template)
(“Use this template”). Never silently write to a live client repo —
propose a PR unless I explicitly ask to apply it.

Personal portfolio, resume, and social posting are **out of scope**.

### Related repos

| Repo | Role |
|---|---|
| `aftongauntlett/pppg-tooling` (this repo) | Studio automations, agent rules, intake glue |
| [`aftongauntlett/template`](https://github.com/aftongauntlett/template) | Public GitHub template for new client sites (Astro, WCAG 2.2) |
| `aftongauntlett/prettyprettyprettygood` | Studio marketing site (not the client starter) |
| Live client repos (`rcan`, `astrid-beauty`, …) | Production sites; do not fork these as the template |

---

## Part 1: Email + Linear (client tracking)

### Goal

A client email (new inquiry or an existing client's request) becomes a
Linear **issue**. Site work, if any, happens on the **client repo**.
I approve merges and I send mail. Nothing auto-merges or auto-sends.

### Linear — already connected

Cursor ↔ Linear is **already authorized** for the PPPG team. Agents
can be kicked off with `@Cursor` in a Linear comment, by assigning
Cursor, or later with a Cursor Automation on **Issue created** /
**Status changed**.

Still needed **in Linear itself** (not another Cursor OAuth):

- A `Needs approval` workflow state for merge-approval and
  email-approval gates
- A saved filtered view of that state (the one place I check)
- Optional: Linear’s own digest on that state — no custom reminder bot
- An explicit `client → git repo` map (this repo, Linear project
  fields, or a `repo` label group with `owner/repo` children) before
  any “go edit the site” automation. Agents must not guess the repo.

### Slack — already connected

Cursor ↔ Slack is **already integrated**. Useful as a second intake
surface (paste a request, `@Cursor` in a channel) and for “needs
approval” pings. Slack does **not** replace Linear as the system of
record. Do not auto-post client email contents into public channels.

### Mailbox: Neo

Inquiry inbox: `info@prettyprettyprettygood.org` on Neo Mail. Keep it.

Neo has no mailbox REST API and no “new mail” webhooks. Two doors:

| Door | What it is good for |
|---|---|
| **Neo MCP** (`https://api.neo.space/mcp`) | An agent **in a session** can search, read, label, and (with my approval) send mail. |
| **IMAP** | Zapier/Make “new email” → Linear issue. Fire-and-forget intake. |

[Neo’s MCP docs](https://support.neo.space/hc/en-us/articles/60120952867481-Neo-MCP-Connect-Neo-Mail-with-Claude-and-ChatGPT)
only list Claude and ChatGPT (`OAuth Client ID` `claude` / `chatgpt`).
Cursor speaks remote MCP + OAuth but is not a documented Neo client.

**Can Claude set up email and push the changes?** Partly.

- Asking **Claude on claude.ai** to add Neo MCP follows Neo’s official
  guide. That only helps **Claude chats**. It does not connect Cursor,
  does not create Linear issues, and does not push git.
- Asking **Claude in Cursor** can add a `.cursor/mcp.json` stub for
  `https://api.neo.space/mcp` and can write Zapier/automation notes
  into this repo. I still have to complete Neo OAuth in the browser
  (Cursor Settings → MCP, and the MCP dropdown on
  [cursor.com/agents](https://cursor.com/agents) for cloud agents).
  Claude cannot create a live Zapier zap or click OAuth.
- Do not commit the mailbox password. Do not “always allow” send-mail.

Prefer MCP OAuth (2FA can stay on). IMAP is the fallback and requires
Neo **Enable on other apps**; Neo **2FA blocks IMAP** (no app
passwords).

IMAP fallback: `imap0001.neo.space:993` SSL/TLS. SMTP
`smtp0001.neo.space:587` STARTTLS or `:465` SSL/TLS. Full email as
username. Do not use POP.

MCP is **select Neo plans only**, and it is **not** a new-mail
trigger. Intake still needs Zapier/Make IMAP **or** a scheduled Cursor
Automation that lists unread via MCP.

### v1 pipeline

1. New inquiry mail → Linear issue (Zapier/Make IMAP, or a scheduled
   Cursor Automation using Neo MCP once it works). Sender, subject,
   short excerpt. Do not auto-reply.
2. I triage: new inquiry vs existing-client change vs question.
3. Site change → agent on **that client repo**. Prefer a **draft PR**
   for unattended/email-triggered work. Local “I’m at the keyboard”
   edits on `main` are fine — the template already uses main +
   pre-commit hooks.
4. Linear issue → `Needs approval`.
5. I merge or reject (or I already committed locally).
6. Issue → Done, **draft** a reply. I send from Neo.

One agent + CI is enough. No extra review/test agents unless I ask.

**Not v1:** SMTP auto-send, unattended repo selection, three-agent
chains.

---

## Part 2: Client websites (`aftongauntlett/template`)

**Use the existing public template.** Do not create `pppg-template`
and do not clone `sveltekit-starter`.

[`aftongauntlett/template`](https://github.com/aftongauntlett/template)
is already an Astro 7 GitHub template aimed at PPPG work:

- “Use this template” → new client repo with unrelated history
- `PROJECT_BRIEF.md` as the intake / kickoff input
- WCAG 2.2 AA in docs + eslint `jsx-a11y`
- Tokenized CSS, layout + UI primitives, Home + Example catalog
- Agent modes: site-builder (cloned site) vs template-maintainer
- `npm run detach-template` strips maintainer-only docs on new sites
- `npm run validate` (typecheck, lint, tests, build) and pre-commit
  hooks for solo `main`

Each client: GitHub **Use this template** (or
`gh repo create --template aftongauntlett/template`), fill
`PROJECT_BRIEF.md`, run the new-site-kickoff prompt.

**Keep evolving this template** rather than starting over. Gaps vs the
quality bar on `rcan` (add in the template repo when we care, not
here):

- CI runs on `push` to `main` only — add `pull_request` if agents open
  PRs
- No Playwright + axe e2e, no Lighthouse CI
- No Formspree / Turnstile in the starter (add when a site needs a
  form)
- Site-builder mode forbids new components by default. “Original
  industry layout” that needs new primitives belongs in
  **template-maintainer** first, then the cloned site, unless I
  override.

Lock to **Astro**. A database app / mobile app / game is a different
starter later.

Placeholder photos: Pexels / Unsplash **APIs**, attribution in the
repo. No logo → typographic lockup over an AI mark.

---

## Connections

| Integration | Status |
|---|---|
| Linear ↔ Cursor (PPPG team) | **Done** |
| Slack ↔ Cursor | **Done** |
| GitHub | Connected (this repo; more org/client repos may be on the environment — a new agent run sees those) |
| Neo MCP in Cursor / cloud agents | **Not done** — I click OAuth |
| Neo MCP on claude.ai | Optional, separate OAuth, does not drive this pipeline |
| Zapier/Make IMAP → Linear | **Not done** — needed for fire-and-forget intake |
| Linear `Needs approval` + filtered view + client→repo map | **Not done** — Linear UI |

Agents cannot finish Neo OAuth, Zapier, or Linear workflow-state
clicks. They can use Linear and Slack once a session has those tools,
and they can draft git changes (including an MCP stub) for me to
authorize.

---

## Build order

1. **Mail → Linear issue** (Neo MCP in Cursor if OAuth works;
   otherwise Zapier IMAP). Linear states/view/map in parallel.
2. **Client sites** live in repos created from `aftongauntlett/template`.
   Template improvements happen in that repo, not here.

Do not implement portfolio/resume sync, social posting, SMTP
auto-send, or a second website template.
