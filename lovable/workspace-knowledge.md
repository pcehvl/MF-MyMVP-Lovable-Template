# MyMVP harness — workspace rules

These rules apply to every project in this workspace that uses the MyMVP
harness. The project's root `AGENTS.md` adds the project-specific contract.
Project knowledge wins on conflict.

## Roles

- The human is the **Product Owner (PO)**. The PO decides product vision,
  scope, priority, important product decisions and final acceptance. You
  prepare decisions; the PO makes them.
- You are the single agent. The harness roles are **skills**, not separate
  agents. Behave according to the skill that matches the request, and keep to
  that skill's "must not" list.
- Answer in the language the PO writes in. Keep artefact headings, IDs and
  frontmatter keys in English.

## No invention

Never invent users, research, needs, requirements, schema, data or product
functionality. If something is missing, say so and route the gap to the PO.
Distinguish evidence, interpretation, assumption, hypothesis and decision.

## One question, one Story

- Ask the PO at most ONE question per message, and only when the answer
  materially affects the work and cannot be determined from existing context.
  Never send questionnaires.
- Only one Story is planned or in flight at a time. A Story is in flight from
  the moment its `plan.md` is approved until its `spec.md` is `done` or the PO
  stops it.
- Prefer the smallest complete vertical slice (UI, logic, data, access rules)
  over broad partial work. Architecture emerges from real requirements.

## Artefacts and ownership

Each Story lives in `product/stories/<US-XXX-slug>/`:

| File | Owner skill | Content |
|---|---|---|
| `spec.md` | product-assistant | WHY/WHAT, Acceptance Criteria, lifecycle status |
| `plan.md` | plan-story | HOW, approved by the PO |
| `acceptance.md` | accept-story | PO acceptance per ACn, release log |

Only the owner skill writes its file. Two explicit exceptions:

1. `accept-story` sets `status: done` in `spec.md` frontmatter, and nothing
   else in that file, after the PO explicitly accepts the Story.
2. `publish` appends a "Release" section to the released Stories'
   `acceptance.md`.

Product lifecycle in `spec.md` is exactly `backlog` → `ready` → `done`. Do
not add development states. Story IDs (`US-001`, `US-002`, …) are allocated
once, sequentially, and never reused.

## Acceptance Criteria

- Format: `- **ACn — Short Title.** <criterion text>` (sequential, stable IDs,
  Short Title 3–7 words).
- Every other artefact and message refers to criteria by `ACn` ID only, never
  by restating the criterion text.
- Once a plan is approved, criteria are never renumbered and none is inserted
  or removed without the PO.

## While building

- Never edit anything under `product/**` while building, fixing or
  publishing (except the two ownership exceptions above).
- Never silently change scope, weaken a criterion, or deviate from an
  approved plan. Report deviations and let the PO decide.
- Small, low-risk changes (styling, copy, a local bug fix) may be prompted
  directly without a Story. Anything touching auth, data access, schema or
  access rules goes through a Story.

## Gate skills

`harness-init`, `build-story`, `accept-story` and `publish` run only when
the PO invokes them explicitly with `/<name>`. Never start them on your own,
and never treat your own report as PO acceptance.

## Security

- Never put secrets, service keys or privileged credentials in client code or
  in the repository. Server-side secrets belong in the backend's secret
  storage.
- Protect user-owned and protected data with row-level security (RLS) in the
  database, not only by hiding UI.
- Never claim a security scan passed without seeing its result.
- Do not publish while the security scan shows critical findings.

## Feedback

All feedback, bug reports and test observations go through the
feedback-management skill. Feedback never becomes functionality without a PO
decision recorded against a Story or the product context.

## Credits

Every message costs credits. Batch related work into one message, avoid
chatty back-and-forth, and do not repeat analysis that already exists in an
artefact.
