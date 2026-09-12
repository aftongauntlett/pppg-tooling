# pppg-tooling — Automation Spec

## Context

I run a small pro-bono/client web studio (Pretty Pretty Pretty Good). I mostly
build static marketing sites for local service businesses (lawyers,
therapists, auto body shops, salons) — no backend or database work
currently, but that could change per-client.

This repo is **internal tooling and automation only**. It is not a client
website starter. Never clone it to start a client job.

It should never silently modify my portfolio, resume, or live client
repos. Read from them where needed; treat writes as a proposal (usually a
PR) unless I explicitly ask to apply them.

### Related repos

| Repo | Role |
|---|---|
| `aftongauntlett/pppg-tooling` (this repo) | Studio automations, agent rules, intake pipeline glue |
| `aftongauntlett/pppg-template` (to be created) | GitHub template for new client sites |
| `aftongauntlett/react-portfolio` | Personal portfolio / job history source for resume sync |
| `aftongauntlett/resume` | JSON → pdfmake resume; 2-page PDF |
| `aftongauntlett/prettyprettyprettygood` | Studio marketing site (not the client starter) |
| Live client repos (`rcan`, `astrid-beauty`, …) | Production sites; never used as the template itself |

Social posting is **out of scope**. Do not draft or publish Bluesky,
Facebook, or other social posts.

---

## Part 1: Portfolio → resume sync

When a new job / project entry is added to `react-portfolio`, update
`resume` so `resumeData.json` reflects the same work.

The resume repo already owns PDF generation (`npm run build` →
`afton-gauntlett-resume.pdf` via pdfmake). After a successful resume
build **on my machine**:

- Copy `afton-gauntlett-resume.pdf` to `~/Desktop/afton-gauntlett-resume.pdf`
- Replace the file if it already exists (`copyFileSync` overwrite is
  correct; do not version-stamp the Desktop filename)

Today, `build.js` auto-copies **cover letters** to Desktop but not the
resume PDF. Resume Desktop copy is the documented operator workflow
(README Option B). When this automation is implemented, add the same
Desktop copy to the resume build path.

**Hard constraint:** the PDF must stay **2 pages**. The resume repo
already checks this with `pdfinfo`. If adding content would exceed 2
pages, condense older / less relevant entries rather than dropping the
new one silently — flag the tradeoff to me. Do not ship a 3-page PDF.

**Cloud-agent caveat:** a cloud VM does not have my Mac Desktop. Cloud
runs may update `resumeData.json`, rebuild the PDF, and open a PR on
`resume`. The Desktop replace only happens when `npm run build` runs
locally (or via a later local hook). Do not treat a cloud `~/Desktop`
copy as success.

Approval: propose a PR on `resume` (and only touch `resumeData.json` plus
regenerated PDF as needed). Do not push to `resume` `main` unattended.

---

## Part 2: Client website template

**Architecture (decided):** a separate GitHub **template** repo,
`pppg-template`, that I create. Each new client is
“Use this template” / `gh repo create --template`, then an agent fills
it from the intake form. Do not clone `pppg-tooling` per client.

### Existing starter — reuse or start fresh?

Reviewed `aftongauntlett/sveltekit-starter` (the existing personal
starter) plus recent production sites `rcan` and `astrid-beauty`.

**Start `pppg-template` fresh in Astro. Do not clone `sveltekit-starter`
as the client template.**

| Source | Verdict |
|---|---|
| `sveltekit-starter` | Nice a11y + HEX→HSL theme-sync ideas, but **SvelteKit**, last touched 2025-07, clone-not-template workflow. Wrong framework for current PPPG work. |
| `rcan` | Closest **quality bar** (Astro 7, Tailwind 4, axe-core, Lighthouse CI, Playwright, Formspree + Turnstile). Live client site with admin/change-request extras — extract the toolchain, do not fork the repo. |
| `astrid-beauty` | Production Astro + React islands + i18n. Too client-specific; Astro 5. Steal patterns, not the tree. |
| `prettyprettyprettygood` | Studio site, not a generic client starter. |

Steal from `rcan` for the template’s CI/quality floor: `astro check`,
eslint `jsx-a11y`, Playwright + axe, Lighthouse CI, Formspree + Turnstile
for contact. Keep the template **static-first** (no RCAN admin dashboard
unless a client actually needs it).

**Framework (decided):** lock the template to **Astro**. Do not keep it
framework-agnostic. A database app, mobile app, or game is a different
starter, not a flag on this one.

**Goal:** open the new client repo, drop in one prompt plus the filled
intake form, get a largely presentable static site in one pass.
Refinement after that is expected.

**Requirements:**

- Accessibility is non-negotiable: WCAG 2.2 AA and Section 508 **baked
  into CI**, not only a prompt reminder.
- Design tailored to the client's industry — reference common patterns
  for that industry and generate an original layout in that spirit, not
  a generic template reskin.
- If no logo is provided, prefer a simple typographic lockup over an
  AI-generated mark. Flag that generated image-logos are weak IP.
- If no color theme is provided, choose one appropriate to the industry.
- Always generate a custom favicon; never ship the framework default.
- Source free, license-safe photography (Pexels / Unsplash **APIs**)
  when the client hasn't supplied images. Store attribution in the repo.

**Client intake:**

- A reusable intake form (markdown is fine) lives in this tooling repo
  and/or the template.
- I send it to new clients; the filled form is the agent's input.

**Future (not v1):** structured fields from email / the form land on the
Linear issue automatically.

---

## Part 3: Email intake → Linear → site work

### Mailbox (decided): keep Neo

The inquiry inbox is Neo Mail (`info@prettyprettyprettygood.org`), not
Google Workspace. Neo is Titan-branded at the provider layer (same IMAP
product; support even CCs `support@titan.email`). Do not migrate this
mailbox for automation.

| | |
|---|---|
| IMAP | `imap0001.neo.space:993` SSL/TLS |
| SMTP | `smtp0001.neo.space:587` STARTTLS or `:465` SSL/TLS |
| Auth | full email address + mailbox password |
| POP | do not use (pulls mail off the server) |

Neo has **no mailbox REST API and no “new mail” webhooks**. IMAP + SMTP
are enough.

**Neo setup I must do in webmail (agents cannot click this):**

1. Settings → **Enable Neo on other apps** (third-party IMAP).
2. 2FA **blocks IMAP**. Neo has no Google-style app passwords. For this
   isolated inquiry seat, disable 2FA on that mailbox only.

### Linear

Tracked items are **issues**, with **workflow states**, on a team.

Needed in the PPPG Linear team:

- A `Needs approval` state (or equivalent) for merge-approval and
  email-approval gates
- A saved filtered view of that state (the one place I check)
- Optional: Linear’s own notification / digest on that state — do not
  build a custom reminder bot in v1

Cursor can already run cloud agents from Linear (`@Cursor` in a comment,
assign to Cursor, or a Cursor Automation on **Issue created** /
**Status changed**). That requires the Linear integration connected in
the Cursor dashboard — see [Connections](#connections-agents-cannot-finish-these).

### v1 pipeline (decided)

Keep v1 small. Prefer Zapier/Make before custom agent mail polling.

1. **New IMAP email** in the Neo inquiry inbox → Zapier/Make → create a
   Linear **issue** summarizing the request. Include sender, subject,
   and a short body excerpt. Do not auto-reply.
2. I triage: new inquiry vs existing-client change vs question. Agents
   do **not** pick the git repo unattended. Maintain an explicit
   `client → repo` map (in this tooling repo or Linear project fields)
   before any “go edit the site” automation.
3. If it is a site change: start a cloud agent **on that client repo**.
   It opens a **draft PR**. The PR is the commit/approval gate — do not
   work unattended on `main`.
4. Move the Linear issue to `Needs approval`.
5. I review and merge (or reject) the PR.
6. After merge, move the issue to Done and **draft** a client reply
   (Linear comment and/or IMAP draft). **Never send mail without my
   explicit approval.** Prefer I send from Neo webmail.

Do not stand up separate review-agent + test-agent runs per request.
One agent + CI on the PR (including a11y checks) is enough for static
sites. If a change is risky, flag it in the Linear issue.

Nothing in this pipeline auto-merges or auto-sends, regardless of
confidence.

**Not v1:** IMAP polling from a Cursor cron, Neo MCP as the intake
trigger, SMTP send from an agent, working on `main`, three-agent review
chains.

Neo’s MCP (`https://api.neo.space/mcp`) is useful later for interactive
“summarize this thread” while I am in a session. It is **not** an
event-driven new-mail trigger.

---

## Connections (agents cannot finish these)

Neither this cloud agent nor another Grok/Cursor run can complete
OAuth, click Neo settings, or create a Zapier zap. Those need my
browser. Once they exist, later agents can *use* them.

### Linear ↔ Cursor (required before any Linear-triggered agent)

1. [Cursor Integrations](https://cursor.com/dashboard/integrations) →
   **Connect** next to Linear.
2. Authorize the PPPG Linear workspace and team.
3. Confirm GitHub is connected (agents open PRs as me / Cursor).
4. Optional but useful: in Linear, a `repo` label group with
   `owner/repo` children (`aftongauntlett/rcan`, etc.) so issues can
   name their client repo.
5. Optional: a Cursor Automation at
   [cursor.com/automations](https://cursor.com/automations) with trigger
   **Linear → Issue created** (or **Status changed**), after v1 mail→
   Linear is reliable. Do not auto-run code changes on every new issue
   until the `client → repo` map exists.

A Linear API key in Cursor secrets would let an agent *create* issues
from a custom script. That is unnecessary if Zapier/Make creates the
issue.

### Neo mail → Linear (v1)

1. Finish the Neo third-party / 2FA steps above.
2. In Zapier or Make: IMAP trigger on
   `imap0001.neo.space:993` with the inquiry address + password.
3. Action: Linear “Create issue” on the PPPG team.
4. Keep the mailbox password in Zapier/Make, not in this git repo and
   not in a cloud-agent snapshot unless we later have a strong reason.

### What an agent *can* do after that

- Subscribe to Linear issue events in a running session (team/project/
  issue scope)
- Open PRs on a repo it was started against
- Draft reply text in Linear comments
- Read this spec and follow the approval gates

It still cannot: log into Neo as me, authorize Linear, or save a Cursor
Automation from inside a coding session.

---

## Build order

1. **Part 1** — portfolio → resume PR + 2-page PDF check; Desktop copy
   on local builds. Low-stakes, uses repos that already exist.
2. **Part 3** — Neo IMAP → Linear issue only. Site-edit automations
   wait until the `client → repo` map and Linear↔Cursor connection
   exist.
3. **Part 2** — `pppg-template` (I create the empty GitHub template;
   implementation can proceed in that repo in parallel with 1–2, but
   do not block intake work on template polish).

Do not implement social, SMTP auto-send, or framework-agnostic
templates.
