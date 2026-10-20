# AGENTS.md

This Lovable project runs the **execution loop** of the MyMVP harness. Specs
come from the team's product workspace (OpenCode) as handovers; you plan,
build and publish them with the Product Owner (PO). The workspace knowledge
holds the universal rules.

## Sources of truth

| Artefact | Location | Written by |
|---|---|---|
| Product context (copy) | `product/product.md` | receive-story |
| Story spec (copy) | `product/stories/<US-XXX-slug>/spec.md` | receive-story |
| Implementation plan | `product/stories/<US-XXX-slug>/plan.md` | plan-story |
| Acceptance record | `product/stories/<US-XXX-slug>/acceptance.md` | accept-story, publish |
| App code, schema, RLS | the app itself | build-story |

The copies are read-only. Never edit `product.md` or a `spec.md` except
through `receive-story`.

## How to work here

1. A handover arrives (via the product workspace, or pasted) → receive-story.
2. Plan mode: "plan US-XXX" → plan-story proposes a plan for the received revision.
3. PO approves → "save the plan" → `plan.md` (`status: approved`, `spec_revision`).
4. Build mode: `/build-story US-XXX` → implement, self-check each ACn, report.
5. PO checks the preview → `/accept-story US-XXX` → `acceptance.md`.
6. Any FAIL → fix prompt citing the ACn → back to step 4.
7. `/publish` → precondition check → the PO clicks Publish.
8. New ideas or changed requirements → not built here; noted under "New
   wishes" in `acceptance.md` and taken back to the product workspace.

## Definition of Done (here)

A Story is accepted when every Acceptance Criterion passes on the preview
for the received revision, the PO has explicitly accepted it in
`/accept-story`, and the security scan shows no critical findings. The
product workspace then marks it `done`.
