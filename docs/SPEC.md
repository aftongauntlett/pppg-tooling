# pppg-tooling — Automation Spec

## Context

I run a small pro-bono/client web studio (Pretty Pretty Pretty Good). I mostly
build static marketing sites for local service businesses (lawyers,
therapists, auto body shops, salons) — no backend or database work
currently, but that could change per-client.

This repo is for internal tooling and automation only. It should never
directly modify my portfolio, resume, or live client repos — read from them
where needed, but treat writes to those repos as something to propose, not
execute silently.

---

## Part 1: Cross-repo automation

### 1a. Portfolio → Resume sync

- When a new job entry is added to my portfolio repo, update my resume
  repo to reflect the same addition.
- Hard constraint: resume must never exceed 2 pages. If adding content
  would exceed that, condense older/less relevant entries rather than
  dropping the new one silently — flag the tradeoff to me.

### 1b. New project → social post

- When a new client project goes live (defined as: added to the
  "projects" section of my main site), generate a social post announcing
  it, e.g. "Check out the new site I just built: [link]"
- Target platforms: Bluesky and Facebook.
- Draft for my review before publishing. Do not auto-post without a
  check unless I explicitly change this later.

---

## Part 2: Client website template system

Evaluate whether a cloneable template repo is the right architecture for
this, or propose a better approach if not.

**Goal:** A repo I can open per new client, drop in one prompt containing
the client's info, and get a largely complete static website in one pass.
Some back-and-forth refinement after is fine, but the first output should
already be presentable.

**Requirements:**

- Accessibility is non-negotiable: WCAG 2.2, Section 508 compliance built
  in by default, not optional.
- Design tailored to the client's industry — reference common design
  patterns for that industry and generate an original layout in that
  spirit, not a generic template reskin.
- If no logo is provided, generate one fitting the industry and theme.
- If no color theme is provided, choose one appropriate to the industry.
- Always generate a custom favicon; never ship the framework default.
- Source free, license-safe placeholder photography (e.g. Pexels,
  Unsplash) automatically when the client hasn't supplied their own images.

**Client intake:**

- Generate a reusable intake form (`.md` or similar) I can send to new
  clients to collect what I need.
- The filled-out form becomes the input the agent uses to build the site.

**Framework decision:**

- Currently using Astro. Advise whether that's still the right choice or
  something else fits better.
- Open question: lock the template repo to one framework, or keep it
  framework-agnostic (rules/skills only) so it can flex into a
  database-backed app, mobile app, or game if a future client needs that?
  Weigh in given I'm a solo operator, not a team.

---

## Part 3: Email intake → full client-request pipeline

**Setup context:**

- Isolated inbox on Titan (not personal email), used only for PPPG
  client inquiries. Titan was chosen over Google Workspace for cost.
  **Before building anything else in this section**, investigate Titan's
  actual integration options: native API, IMAP/SMTP access, webhook
  support, or third-party connector compatibility (Zapier/Make/etc). If
  direct automation isn't reasonably supported, propose a workaround
  (e.g. polling via IMAP on a schedule) or, if Titan genuinely can't
  support this reliably, say so plainly and suggest an alternative
  (e.g. a cheap Google Workspace mailbox used only for automation,
  keeping Titan for anything that doesn't need to be automated) rather
  than forcing something fragile.
- Linear account for PPPG client/project tracking.

**Goal:** A client email (new inquiry or an existing client's request/
question) triggers an end-to-end pipeline, with explicit approval gates
at the points that matter.

**Pipeline:**

1. Incoming client email is read and a Linear issue (or whatever Linear
   calls its tracked item — confirm current terminology) is created,
   summarizing the request.
2. Work is completed directly on main (no feature branch — solo operator,
   branch/merge overhead isn't worth it here; skip straight to work in
   main unless a specific task seems risky enough to warrant isolating it,
   in which case flag that to me rather than deciding silently).
3. A separate review agent reviews the change.
4. A separate agent confirms tests pass.
5. Changes are committed — **requires my explicit approval before
   committing, always.**
6. Once approved and committed, the Linear issue is moved to Done.
7. A reply email to the client is drafted — **requires my explicit
   approval before sending, always.**

**Approval gates (non-negotiable):**

- Committing to main: my approval required, every time.
- Client-facing email: my approval required, every time.
- Nothing in this pipeline should auto-commit or auto-send under any
  circumstance, regardless of how minor the change or how confident the
  review/test agents are.

**Tracking what needs my attention:**

- Set up a Linear workflow state (e.g. "Needs Approval") that issues move
  into automatically whenever they're sitting at a merge-approval or
  email-approval gate.
- Save a filtered Linear view scoped to that state so I have one place to
  check for anything waiting on me, rather than needing to track it
  manually or dig through general activity.
- Consider whether a Linear notification/reminder tied to that state
  would help surface it further (e.g. daily digest), without becoming
  something I need to babysit constantly.

**Future extension (not v1):** extract structured fields (services
requested, business type, etc.) into the Linear card automatically, tying
into the Part 2 intake form.

---

## Build order

Sequence: Part 1 → Part 3 → Part 2.

Parts 1 and 3 are small, contained, and low-risk — good for validating the
harness and building trust in unsupervised runs. Part 2 is a much bigger
surface area (design decisions, image sourcing, framework flexibility) and
should wait until the smaller pieces are working.
