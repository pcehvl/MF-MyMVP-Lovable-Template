# AGENTS.md

This project uses the MyMVP harness. The workspace knowledge holds the
universal rules; this file binds them to this project. The human is the
Product Owner (PO) and decides scope, priority and acceptance.

## Sources of truth

| Artefact | Location | Owner skill |
|---|---|---|
| Product context | `product/product.md` | product-assistant |
| Product Backlog | `product/stories/` (one directory per Story) | product-assistant |
| Story requirements | `product/stories/<US-XXX-slug>/spec.md` | product-assistant |
| Implementation plan | `product/stories/<US-XXX-slug>/plan.md` | plan-story |
| Acceptance record | `product/stories/<US-XXX-slug>/acceptance.md` | accept-story |
| Discovery material | `docs/design-thinking/` | product-assistant |
| App code, schema, RLS | the app itself (Lovable) | build-story |

Do not create competing sources of truth (no `backlog.md`, no issue tracker).
Exceptions to single ownership: `accept-story` sets `status: done` in
`spec.md`; `publish` appends a Release section to `acceptance.md`.

## Lifecycle

- `spec.md`: `backlog` → `ready` (PO confirms) → `done` (PO accepts).
- `plan.md`: `status: approved` once the PO approves it.
- One Story in flight at a time.

## How to work here

1. Plan mode: talk freely about the product → product-assistant.
2. Build mode: "save" → `spec.md`; status `ready` only after the PO confirms.
3. Plan mode: "plan US-XXX" → plan-story proposes a plan.
4. PO approves → Build mode: "save the plan" → `plan.md` (`status: approved`).
5. Build mode: `/build-story US-XXX` → implement, self-check each ACn, report.
6. PO checks the preview → `/accept-story US-XXX` → `acceptance.md`.
7. Any FAIL → fix prompt citing the ACn → back to step 5.
8. `/publish` → precondition check → the PO clicks Publish.
9. `/feedback-management` → classify → PO decides what happens.
10. Small low-risk changes may be prompted directly, without a Story.

## Definition of Done

A Story is done only when every Acceptance Criterion passes on the preview,
the PO has explicitly accepted it in `/accept-story`, and the security scan
shows no critical findings. Generated code alone is never done.
