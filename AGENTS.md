# AGENTS.md

Product workspace for spec-driven MVP development. This repository holds
the product loop: evidence, product framing, backlog and Story specs. The app
is built in Lovable, from the specs handed over from here.

The human is the **Product Owner (PO)**. The PO decides product vision,
scope, priority and acceptance. The `product-assistant` agent (default)
prepares those decisions.

## Two loops, two tools

```text
PRODUCT LOOP (here, OpenCode)
Evidence → Framing → Backlog → Story → Refinement → ready (rev N)
  → /handover US-XXX ─────────────────────────────┐
                                                  ▼
EXECUTION LOOP (Lovable, driven by the PO in the Lovable UI)
receive → plan → approve → build → accept on preview → publish
  ┌───────────────────────────────────────────────┘
  ▼
/return US-XXX → done, or feedback → Story revision or new Story
```

Only two things cross between the loops: a **handover** (product.md + one
ready spec.md) and a **return** (that Story's acceptance.md). Method:
`lovable-bridge` skill.

## Sources of truth

| Artefact | Location | Written by |
|---|---|---|
| Evidence cards | `docs/design-thinking/E-xxx-*.md` | product-assistant |
| Product context | `product/product.md` | product-assistant |
| Product Backlog | `product/stories/` (one directory per Story) | product-assistant |
| Story spec | `product/stories/<US-XXX-slug>/spec.md` | product-assistant |
| Acceptance record (copy) | `product/stories/<US-XXX-slug>/acceptance.md` | `/return`, from Lovable |
| Linked Lovable project | `product/lovable-project.md` | `lovable-bridge` |

Implementation plans, app code, database and the original acceptance record
live in Lovable. Do not create competing sources of truth (no `backlog.md`,
no issue tracker).

## Spec-driven rules

- **The spec is the contract.** Lovable builds only what a handed-over spec
  says. Every change of behaviour starts as a spec change here.
- **Revisions.** A `ready` Story carries `revision: N`. Any change of meaning
  bumps it and requires a new handover; Lovable then re-plans.
- **Acceptance Criteria** are written `- **ACn — Short Title.** <text>` and
  referred to by `ACn` ID everywhere else.
- **Done** is set here, only after a return shows the Story accepted against
  the current revision and the PO confirms it. A `done` Story stays done; a
  later change becomes a new Story.
- **Feedback** (including "New wishes" in a return) never becomes
  functionality without a PO decision.

## Rules that always apply

- **One question at a time.** At most ONE question per message, and only
  when the answer materially affects the work and cannot be determined from
  existing context.
- **No invention.** Never invent users, research, requirements or product
  functionality. Record gaps as open questions.
- **`docs/examples/` is fictional teaching material.** Use it only to show
  the format; never treat it as evidence or as part of this product.
- **Lovable access is limited** to the tools in the `lovable-bridge` skill.
  Never ask Lovable to build, fix, publish or query data from here.

## Commands

| Command | Does |
|---|---|
| `/handover US-XXX` | Sends product.md + the Story's spec.md to Lovable and checks they arrived |
| `/return US-XXX` | Fetches the Story's acceptance.md from Lovable and proposes the next step |
