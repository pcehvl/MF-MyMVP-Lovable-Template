# MyMVP for Lovable — spec-driven

A harness for learning **spec-driven development** with two tools:

- **OpenCode** runs the **product loop**: evidence, product framing, backlog
  and Story specs with Acceptance Criteria (`ACn`).
- **Lovable** runs the **execution loop**: plan, build, accept on the preview
  and publish, each driven by the Product Owner (PO) in the Lovable UI.

Only two things cross between them, through Lovable's MCP server:

```text
/handover US-XXX   OpenCode → Lovable   product.md + one ready spec.md (rev N)
/return US-XXX     Lovable → OpenCode   that Story's acceptance.md
```

The spec is the contract: Lovable builds only what a handed-over spec says,
and every change starts as a spec revision in OpenCode.

## What is in this repository

| Path | For | What it is |
|---|---|---|
| `AGENTS.md`, `opencode.json`, `.opencode/` | OpenCode | Product workspace: agent, skills, `/handover` and `/return` |
| `product/`, `docs/design-thinking/` | OpenCode | Product context, Story specs, evidence cards |
| `lovable/skills/` | Lovable | Six execution-loop skills to import into the Lovable workspace |
| `lovable/workspace-knowledge.md` | Lovable | Rules to paste into workspace knowledge |
| `docs/setup.md` | everyone | Setup and daily use |
| `docs/harness-design.md` | teachers | Design, decisions and caveats |
| `docs/test-plan.md` | teachers | Tests to run before using it with students |

## Quick start

**Teacher, once per Lovable workspace:** import each folder under
`lovable/skills/` as a skill, set auto-use OFF for `harness-init`,
`build-story`, `accept-story` and `publish`, and paste
`lovable/workspace-knowledge.md` into workspace knowledge.

**Student, once per product:**

1. On GitHub: **Use this template** → your own product repository → clone it.
2. Open the folder in OpenCode, run `/mcps` and sign in to `lovable`.
3. In Lovable: create a new project and run `/harness-init` in Build mode.

**Student, every Story:**

```text
OpenCode  frame → write the Story → ready → /handover US-XXX
Lovable   plan US-XXX → approve → /build-story US-XXX → check preview → /accept-story US-XXX
OpenCode  /return US-XXX → done, or feedback → new revision → /handover again
Lovable   /publish when you want to release
```

Details: [`docs/setup.md`](docs/setup.md).

Derived from [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template).
