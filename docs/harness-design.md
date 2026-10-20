# Harness design

How the MyMVP method maps onto Lovable, and what to watch out for.

## Principles

- The product loop is unchanged: framing, backlog, Story IDs, `ACn`
  Acceptance Criteria, `backlog` → `ready` → `done`, and PO gates.
- Lovable does all execution: planning, building, schema and RLS, fixing and
  publishing.
- Everything is operated from the Lovable UI by prompting. Lovable is one
  agent; the harness roles are skills that run automatically on matching
  prompts, or explicitly with `/<name>`.
- There is no lint, no automated tests and no local check step. The gate is
  the PO's acceptance on the preview.

## Roles → skills

| Role in the method | Skill | Owns | Auto-use |
|---|---|---|---|
| Product Assistant | `product-assistant` (+ `product-management`) | `product/product.md`, `spec.md`, backlog | ON |
| Feedback routing | `feedback-management` | feedback analysis, PO decision | ON |
| Planning | `plan-story` | `plan.md` | ON |
| Building | `build-story` | app code, schema, RLS | OFF (gate) |
| Acceptance | `accept-story` | `acceptance.md`, `status: done` | OFF (gate) |
| Release | `publish` | Release section in `acceptance.md` | OFF (gate) |
| Project setup | `harness-init` | starter files | OFF (gate) |

Two exceptions to single ownership are deliberate: `accept-story` sets
`status: done` in `spec.md`, and `publish` appends a Release section to
`acceptance.md`.

## Where the rules live

| Layer | File | Read |
|---|---|---|
| Workspace knowledge | `lovable/workspace-knowledge.md` (≤10,000 chars) | always, every project |
| Project contract | `AGENTS.md` at the project root (from `harness-init`) | always, every message |
| Methods | `skills/*/SKILL.md` | when a skill is triggered |

Project knowledge wins over workspace knowledge on conflict.

## Gates

| Gate | Who | Recorded in |
|---|---|---|
| Story ready | PO confirms | `spec.md` `status: ready` |
| Plan approved | PO approves | `plan.md` `status: approved`, approver, date |
| Story accepted | PO, per ACn on the preview | `acceptance.md`, `spec.md` `status: done` |
| Release | PO authorises and clicks Publish | Release section in `acceptance.md` |

## Platform facts this design relies on

- A Lovable project cannot be linked to an existing GitHub repository; Git
  sync always creates a new one. This repository is therefore a skill and
  template source, not a project repository.
- Git sync works two-way on one branch at a time. Migration files pushed via
  Git are not run.
- Skills are workspace-level `SKILL.md` folders with a `name` (lowercase,
  digits, hyphens) and a description. They run on `/` or automatically, and
  auto-use can be turned off per skill.
- Workspace and project knowledge (≤10,000 chars each) and the root
  `AGENTS.md` are always included.

## Unverified behaviour (the harness works either way)

| # | Question | How the harness copes |
|---|---|---|
| U1 | Do skills apply in Plan mode, and can Plan mode write files? | Skills draft in Plan mode and write only in Build mode; if writing fails, they ask the PO to switch and say "save". |
| U2 | Does the importer accept several skills in one ZIP? | Setup assumes one ZIP per skill. |
| U3 | Are bundled template files copied verbatim? | `harness-init` re-reads each file, compares it with the source and fixes differences. |
| — | Does the UI refuse to publish with critical security findings? | `publish` checks the scan itself and blocks on critical findings. |

## Known caveats

- **Roles are conventions, not permissions.** Lovable is one agent and can
  technically edit any file. `accept-story` lists the files changed since the
  plan and flags any edits to `product/**`.
- **No regression safety net.** There are no automated tests.
  `accept-story` offers to re-check the ACs of earlier `done` Stories touched
  by the change. Asking Lovable for one smoke check of the critical journey
  before `/publish` is recommended, not required.
- **Credits.** Every Lovable message costs credits, so skills batch work and
  avoid chatty back-and-forth. Acceptance is the exception: one ACn per
  message is deliberate.
- **Migrations.** Only Lovable may author database changes; migrations pushed
  via Git are not run.

## History

This harness is derived from MF-MyMVP-Template, which used OpenCode with
Planner and Coder agents, a local verify pipeline (lint, typecheck, Vitest,
Playwright), per-Story `verification.md` evidence, Story branches with Pull
Requests, and Vercel/Supabase CLI deployment. All of that is replaced here
by Lovable's own build loop, the PO's acceptance on the preview and
Lovable's Publish.
