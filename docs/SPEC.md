# pppg-tooling — Automation Spec

## Context

Pretty Pretty Pretty Good (PPPG) is a solo pro-bono/client web studio.
This repo is **internal studio tooling only**: client tracking, inquiry
email, and the path from a request to a website. It is not a client
site starter and not a personal career/portfolio tool.

Never clone this repo to start a client job. Never silently write to a
live client repo — propose a PR unless I explicitly ask to apply it.

Personal portfolio, resume, and social posting are **out of scope**.

### Related repos

| Repo | Role |
|---|---|
| `aftongauntlett/pppg-tooling` (this repo) | Studio automations, agent rules, intake glue |
| `aftongauntlett/pppg-template` (to be created) | GitHub template for new client sites |
| `aftongauntlett/prettyprettyprettygood` | Studio marketing site (not the client starter) |
| Live client repos (`rcan`, `astrid-beauty`, …) | Production sites; never used as the template itself |

---

## Part 1: Email + Linear (client tracking)

### Goal

A client email (new inquiry or an existing client's request) becomes a
Linear **issue**. Site work, if any, happens on the **client repo** via
a draft PR. I approve merges and I send mail. Nothing auto-merges or
auto-sends.

### Linear

Tracked items are **issues**, with **workflow states**, on the PPPG team.

Needed:

- A `Needs approval` state for merge-approval and email-approval gates
- A saved filtered view of that state (the one place I check)
- Optional: Linear’s own notification / digest on that state — no custom
  reminder bot in v1
- An explicit `client → git repo` map (this repo or Linear project /
  `repo` labels) before any “go edit the site” automation. Agents must
  not guess the repo.

Cursor can run cloud agents from Linear (`@Cursor`, assign to Cursor, or
a Cursor Automation on **Issue created** / **Status changed**) once
Linear is connected in the Cursor dashboard. See
[Connections](#connections-i-click-these-agents-cannot).

### Mailbox: Neo

Inquiry inbox: `info@prettyprettyprettygood.org` on Neo Mail (Titan under
the hood). Keep it. Do not migrate to Google Workspace for automation.

Neo has no mailbox REST API and no “new mail” webhooks. Two usable
doors:

| Door | What it is good for |
|---|---|
| **Neo MCP** (`https://api.neo.space/mcp`) | An agent **in a session** can search, read, label, and (with my approval) send mail. Same for calendar/contacts if we ever need them. |
| **IMAP** | Zapier/Make “new email” → Linear issue. Fire-and-forget intake without an agent sitting open. |

[Neo’s MCP docs](https://support.neo.space/hc/en-us/articles/60120952867481-Neo-MCP-Connect-Neo-Mail-with-Claude-and-ChatGPT)
are written for Claude and ChatGPT only (`OAuth Client ID` = `claude` or
`chatgpt`). Cursor **does** speak remote MCP + OAuth, but Neo has not
published a Cursor client. Plan:

1. **Try Neo MCP in Cursor** (desktop and
   [cursor.com/agents](https://cursor.com/agents) MCP dropdown) as
   `https://api.neo.space/mcp`. Prefer OAuth over storing the mailbox
   password. If Neo rejects Cursor’s redirect / client id, ask Neo
   support (`hello@neo.space`) whether they can allow Cursor — do not
   paste Claude’s `claude` client id and hope.
2. MCP is **not** a new-mail trigger. A connected MCP lets an agent
   read unread mail when asked, or when a **scheduled** Cursor
   Automation polls. It will not fire the instant a lead arrives.
3. Neo MCP is **select plans only**. Confirm the inquiry seat’s plan
   includes it before relying on it.
4. **Send** via MCP still needs my explicit approval. Default Cursor
   MCP tools to ask before send; never “always allow” send-mail.
5. If MCP will not connect, v1 intake is IMAP via Zapier/Make (see
   below). IMAP requires Neo **Enable on other apps**, and Neo **2FA
   blocks IMAP** (no app passwords). Prefer MCP OAuth so we can leave
   2FA on.

IMAP settings (fallback only): `imap0001.neo.space:993` SSL/TLS. SMTP
`smtp0001.neo.space:587` STARTTLS or `:465` SSL/TLS. Username = full
address. Do not use POP.

### v1 pipeline

1. New inquiry mail becomes a Linear issue (Zapier/Make IMAP, **or** a
   scheduled Cursor Automation that lists unread via Neo MCP once that
   works). Include sender, subject, short excerpt. Do not auto-reply.
2. I triage: new inquiry vs existing-client change vs question.
3. Site change → cloud agent on **that client repo** → **draft PR**.
   Do not work unattended on `main`.
4. Linear issue → `Needs approval`.
5. I merge or reject.
6. After merge, issue → Done, and **draft** a reply (Linear comment
   and/or Neo draft). I send from Neo.

One agent + CI on the PR is enough. No extra review/test agents unless
I ask.

**Not v1:** SMTP auto-send, working on `main`, three-agent chains,
unattended repo selection.

---

## Part 2: Client websites (`pppg-template`)

**Architecture (decided):** a separate GitHub **template** repo,
`pppg-template`, that I create. Each client is “Use this template” /
`gh repo create --template`, then an agent fills it from the intake
form.

### Existing starter — start fresh in Astro

Do not clone `sveltekit-starter` as the client template (wrong
framework, last touched 2025-07). Steal the **quality floor** from
`rcan` (Astro 7, Tailwind 4, axe-core, Lighthouse CI, Playwright,
Formspree + Turnstile). Do not fork live client trees (`rcan`,
`astrid-beauty`).

Lock the template to **Astro**. A database app / mobile app / game is a
different starter later.

**Goal:** open the new client repo, drop in one prompt plus the filled
intake form, get a largely presentable static site in one pass.

**Requirements:**

- WCAG 2.2 AA / Section 508 **in CI**, not only a prompt
- Industry-appropriate original layout, not a generic reskin
- No logo → typographic lockup over an AI mark; flag weak IP
- No theme → pick one for the industry
- Always a custom favicon
- Placeholder photos via Pexels / Unsplash **APIs**, with attribution
  stored in the repo

**Intake:** a reusable markdown form in this repo and/or the template.
Filled form = agent input.

**Later (not v1):** structured fields from email/form onto the Linear
issue.

---

## Connections (I click these; agents cannot)

Neither this cloud agent nor another Grok/Cursor run can finish OAuth,
Neo settings, or a Zapier zap.

### Linear ↔ Cursor

1. [Cursor Integrations](https://cursor.com/dashboard/integrations) →
   Connect Linear → authorize the PPPG team.
2. GitHub connected (PRs).
3. Optional: Linear `repo` label group with `owner/repo` children.
4. Optional later: Cursor Automation, Linear **Issue created**, only
   after the client → repo map exists. Do not auto-code every new
   issue.

### Neo MCP in Cursor (preferred mail access)

1. Confirm the Neo plan includes MCP.
2. Add a custom HTTP MCP server:
   - URL: `https://api.neo.space/mcp`
   - Desktop: Cursor Settings → MCP
   - Cloud agents: MCP dropdown on [cursor.com/agents](https://cursor.com/agents)
3. Complete Neo’s OAuth in the browser (email + password → Allow
   Access). Use only **my** Cursor account, not a shared one.
4. Leave send-mail tools on **needs approval**.
5. If OAuth fails because Neo only listed Claude/ChatGPT, write
   `hello@neo.space` and keep IMAP/Zapier as intake.

### Neo IMAP → Linear (intake fallback, or until MCP polling exists)

1. Neo webmail: Enable Neo on other apps. 2FA off on this inquiry seat
   only if IMAP is required.
2. Zapier/Make: IMAP trigger → Linear Create issue.
3. Mailbox password stays in Zapier/Make, not in git.

### What an agent can do after that

- Use Neo MCP to read/search mail (and draft; send only if I approve
  the tool call)
- Subscribe to Linear issue events in a running session
- Open PRs on the repo it was started against
- Draft reply text in Linear

It still cannot: click OAuth, enable Neo IMAP, or save a Cursor
Automation from inside a coding session.

---

## Build order

1. **Part 1** — Linear connected; mail → Linear issue (MCP if it
   works, otherwise Zapier IMAP). No site-edit automations until the
   client → repo map exists.
2. **Part 2** — I create empty `pppg-template`; implementation lives
   in that repo. Can proceed in parallel with Part 1.

Do not implement portfolio/resume sync, social posting, SMTP
auto-send, or a framework-agnostic template.
