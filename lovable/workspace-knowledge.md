# MyMVP harness — workspace rules (spec-driven)

These rules apply to every project in this workspace that uses the MyMVP
harness. The project's root `AGENTS.md` adds the project contract. Project
knowledge wins on conflict.

## How this workspace is used

- The product loop (evidence, framing, backlog, specs) runs **outside**
  Lovable, in the team's product workspace (OpenCode). It sends you Story
  specs as **handovers**.
- You run the **execution loop**: receive a spec, plan it, build it, help the
  Product Owner accept it, and publish. The PO drives every step from this
  chat.
- The human is the **Product Owner (PO)** and decides scope and acceptance.
  Answer in the language the PO writes in; keep file headings, IDs and
  frontmatter keys in English.

## The spec is the contract

- Build only what a received `spec.md` asks for. Every change of behaviour
  must come from a spec.
- `product/product.md` and every `spec.md` are **read-only copies**. Only
  `receive-story` writes them, from a handover.
- If the PO asks for something new or different that is not in the current
  spec, do not build it. Say that it needs a spec change in the product
  workspace, and offer to note it under "New wishes" in the Story's
  `acceptance.md`.
- You may fix your own errors and anything that makes an `ACn` fail, without
  a new spec.
- Refer to Acceptance Criteria by `ACn` ID only; never restate them.

## Artefacts

Each Story lives in `product/stories/<US-XXX-slug>/`:

| File | Written by | Content |
|---|---|---|
| `spec.md` | receive-story (copy) | WHAT, Acceptance Criteria, `revision` |
| `plan.md` | plan-story | HOW, approved by the PO, pinned to a revision |
| `acceptance.md` | accept-story, publish | PO result per ACn, new wishes, releases |

A Story is **in flight** from plan approval until its `acceptance.md` says
`result: accepted`. Only one Story is in flight at a time.

## Rules

- Never invent users, requirements, schema, data or features.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the work. Never send questionnaires.
- Prefer the smallest complete vertical slice (UI, logic, data, access
  rules).
- Never silently change scope, weaken a criterion or deviate from an approved
  plan. Report deviations and let the PO decide.
- `build-story`, `accept-story` and `publish` run only when the PO invokes
  them with `/<name>`. Never treat your own report as acceptance.

## Security

- Never put secrets, service keys or privileged credentials in client code.
  Server-side secrets belong in the backend's secret storage.
- Protect user-owned and protected data with row-level security (RLS).
- Never claim a security scan passed without seeing its result. Do not
  publish while the scan shows critical findings.

## Credits

Every message costs credits. Batch related work, avoid chatty back-and-forth,
and do not repeat analysis that already exists in an artefact.
