# AGENTS.md

## What this repo is

Internal automation tooling for Pretty Pretty Pretty Good (PPPG), a solo
pro-bono/client web studio. See SPEC.md for the full project spec.

## Guardrails

- Never commit secrets, API keys, or tokens directly into this repo. Use
  environment variables and a `.env` file that is gitignored.
- Never directly write to, commit to, or push into my portfolio, resume,
  or any live client repo. If a task requires touching one of those,
  propose the change and stop — do not execute it automatically.
- Do not auto-publish to social media (Bluesky, Facebook). Draft posts
  and leave them for my review.
- Ask before running anything that creates ongoing cost (recurring API
  calls, scheduled jobs, cloud resources) — flag estimated cost first.
- Keep runs short-leash by default. Do not assume long, unsupervised
  execution unless explicitly told this task is cleared for it.

## Working style

- I'm a solo operator, not a team — favor simple, maintainable solutions
  over elaborate multi-service architectures.
- Prefer no-code/low-code integrations (Zapier, Make) for first versions
  of automations before building custom agent-based replacements.
- When a task has an open design decision (e.g. framework choice), lay
  out the tradeoffs and ask rather than picking silently.
- Explain what you built well enough that I can maintain it myself later
  without re-reading all the code — brief inline comments on anything
  non-obvious.

## Build order

Follow the sequence in SPEC.md: Part 1 (cross-repo sync) → Part 3 (email
→ Linear) → Part 2 (client template system). Don't jump ahead to Part 2
without the earlier pieces working.
