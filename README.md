# pppg-tooling

Internal automation tooling for Pretty Pretty Pretty Good (PPPG) — a solo
pro-bono/client web studio.

## What's here

- **SPEC.md** — main spec: portfolio-to-resume sync, new-project social
  post triggers, the client website template system, and the email intake
  → Linear → build → approval pipeline.
- **AGENTS.md** — rules and guardrails for any agent working in this repo
  (approval gates, no auto-commits to client repos, etc).

## Status

Early build. Follow the sequencing in SPEC.md — don't jump ahead to the
client template system before the smaller automations are working.

## Setup

Copy `.env.example` to `.env` and fill in real values. Never commit `.env`.
