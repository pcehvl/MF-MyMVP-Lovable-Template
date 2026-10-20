# Harness design

How the MyMVP method is split between OpenCode and Lovable to teach
spec-driven development, and what to watch out for.

## Decisions

| # | Decision |
|---|---|
| P1 | The product method stays as in MF-MyMVP-Template: framing, backlog, Story IDs, `ACn` criteria, human gates. |
| P2 | Lovable does all execution: planning, building, schema/RLS, fixing, publishing. |
| P3 | No lint, no automated tests, no local build checks. The gate is the PO's acceptance on the preview. |
| P4 (revised) | The product loop runs in OpenCode; the execution loop is driven by the PO in the Lovable UI. OpenCode talks to Lovable through Lovable's MCP server for exactly two things: handing over a spec and fetching its acceptance result. |
| P5 | In Lovable, the harness roles are skills that run on matching prompts or with `/<name>`. In OpenCode, the product role is an agent with skills. |
| P6 | Each Lovable project is prepared with `/harness-init`. |

Defaults chosen: Lovable Cloud as backend (wording works for own Supabase
too); work on `main` with Lovable version history; English artefacts, with
the agents answering in the PO's language.

## Why this split teaches spec-driven development

- The spec is a contract between two parties. If the result is wrong, the
  first question is whether the spec was precise enough.
- Specification and building are visibly separate, and every behaviour
  change starts as a spec revision.
- Lovable refuses work that is not in a received spec, so "just one small
  change" in the app is not possible without going through the spec.

## Responsibilities

| Tool | Agent / skill | Writes | Auto-use |
|---|---|---|---|
| OpenCode | `product-assistant` agent (default) with `product-management`, `feedback-management`, `lovable-bridge` | evidence cards, `product.md`, `spec.md`, acceptance copies, `lovable-project.md` | — |
| Lovable | `receive-story` | `product.md` and `spec.md` copies | ON |
| Lovable | `plan-story` | `plan.md` | ON |
| Lovable | `build-story` | app code, schema, RLS | OFF (gate) |
| Lovable | `accept-story` | `acceptance.md` | OFF (gate) |
| Lovable | `publish` | Release section in `acceptance.md` | OFF (gate) |
| Lovable | `harness-init` | `AGENTS.md`, `product/stories/` | OFF (gate) |

## The bridge

```text
/handover US-XXX  send_message: "/receive-story" + HANDOVER block (product.md + spec.md)
                  read_file: both files read back and compared with the local ones
/return US-XXX    read_file: acceptance.md → saved locally → done or feedback
```

- OpenCode may use only: `list_workspaces`, `list_projects`, `get_project`,
  `send_message`, `get_message`, `list_messages`, `list_files`, `read_file`.
  Everything else (`deploy_project`, `query_database`, `create_project`,
  knowledge and skill editing, …) is denied in `opencode.json`; the agent's
  own permissions allow only the list above, and `send_message` asks first.
- OpenCode permissions use "last matching rule wins", and agent rules are
  appended after global rules.
- Fallback without MCP: the same HANDOVER block is pasted into the Lovable
  chat; the return is pasted back from Lovable's Code view.

## Revisions and the in-flight rule

- `revision` in `spec.md` starts at 1 when a Story becomes `ready` and
  increases with every change of meaning.
- `plan.md` records `spec_revision`; `build-story` refuses when they differ.
- A Story is in flight in Lovable from plan approval until `acceptance.md`
  says `result: accepted`. One Story in flight at a time.
- `done` is set only in OpenCode, after a return shows the Story accepted
  against the current revision. A `done` Story stays done; later changes
  become new Stories.

## Platform facts this design relies on

- Lovable's MCP server (`https://mcp.lovable.dev`) offers `send_message`
  (with optional `plan_mode`), `list_files` and `read_file` (at a git ref),
  among others. OAuth is the only sign-in. `send_message` costs normal
  credits; reading is free. `deploy_project` is refused while critical
  security findings are open.
- OpenCode names MCP tools `<server>_<tool>` and signs in to remote servers
  with OAuth (`/mcps` or `opencode mcp auth <server>`).
- Lovable skills are workspace-level `SKILL.md` folders; auto-use can be
  turned off per skill. Workspace and project knowledge (≤10,000 chars each)
  and the root `AGENTS.md` are always included.
- No GitHub sync is needed between the two tools. The student's product
  repository and the Lovable project (and its own repository, if connected)
  are independent.

## Unverified (see `test-plan.md`)

| # | Question | How the harness copes |
|---|---|---|
| U1 | Do skills apply in Plan mode, and can Plan mode write files? | Plans are drafted in Plan mode and saved in Build mode. |
| U2 | Does the skill importer accept several skills in one ZIP? | Setup assumes one ZIP per skill. |
| U3 | Does Lovable store handed-over files verbatim? | `receive-story` re-reads and compares; `/handover` reads back and compares again. |
| U4 | Does OAuth from OpenCode to Lovable's MCP server work? | Paste fallback. |
| U5 | Do MCP messages appear in the project chat? | The report in OpenCode and the files carry the state either way. |
| U6 | Which `ref` does `read_file` accept for the latest state? | `lovable-bridge` tries `HEAD`, then `main`. |
| U7 | Does `/receive-story` at the start of an MCP message invoke the skill? | `receive-story` also triggers on the HANDOVER block itself. |

## Known caveats

- **Roles in Lovable are conventions, not permissions.** Lovable is one
  agent. `accept-story` flags any change to the copies outside
  `receive-story`.
- **No regression safety net.** `accept-story` offers to re-check earlier
  accepted Stories touched by the change.
- **Credits.** Every Lovable message, including handovers, costs credits.
- **Migrations.** Only Lovable authors database changes.
- **Teacher material in student repositories.** "Use this template" copies
  `lovable/` and `docs/` into each student repository. They are harmless
  there; students can ignore them.

## History

This harness is derived from MF-MyMVP-Template, which used OpenCode with
Planner and Coder agents, a local verify pipeline (lint, typecheck, Vitest,
Playwright), per-Story `verification.md` evidence, Story branches with Pull
Requests, and Vercel/Supabase CLI deployment. In this harness OpenCode keeps
only the product loop; Lovable's build loop, the PO's acceptance on the
preview and Lovable's Publish replace the rest.
