---
name: build-story
description: Use when the PO explicitly invokes /build-story US-XXX to implement a ready Story from its approved plan. Use only when explicitly invoked with /build-story.
---

# Build Story

Implement one Story as the smallest complete vertical slice, following its
approved plan, and report how every Acceptance Criterion is met.

## 1. Preconditions

Confirm before changing anything, and stop with a one-line explanation if any
fails:

- `product/stories/<US-XXX-slug>/spec.md` has `status: ready`
- `plan.md` exists beside it with `status: approved`
- the spec has not changed in a way the plan does not cover (compare with
  `spec_revision`); if it has, ask the PO to re-approve the plan via
  `plan-story`
- no other Story is in flight

## 2. Implement

- Follow the approved plan. Build the smallest slice that satisfies every
  ACn end to end: UI, logic, data, schema and access rules.
- You author all database changes (tables, migrations, RLS policies) inside
  Lovable. Protect user-owned and protected data with RLS.
- Reuse existing components and the project's theme.
- Keep secrets out of client code; use the backend's secret storage.
- Batch the work: implement the whole slice in as few messages as possible.

## 3. Fix your own errors

Before reporting, resolve build errors, runtime errors and console errors you
can see in the preview. Do not hand a broken preview to the PO.

## 4. Self-check every ACn

Check each Acceptance Criterion against the running preview and report a
table:

| ACn | How it is met | Where to look in the preview |
|---|---|---|

Mark any ACn you could not check yourself as "PO to check" and say why.
Your self-check is technical evidence, not acceptance.

## 5. Report

End with, in this order:

1. the ACn table
2. deviations from the plan (or "none"); deviations are reported, never
   silently applied
3. files and areas changed, including schema and RLS changes
4. the security scan status if visible, and any known limitations
5. one line: "Check the preview, then run `/accept-story US-XXX`."

## 6. Small changes without a Story

Styling, copy and small local bug fixes may be prompted directly without
this skill. Anything touching auth, data access, schema or access rules goes
through a Story.

## 7. Must not

- Do not edit anything under `product/**` (`spec.md`, `plan.md`,
  `acceptance.md` and `product.md` are read-only here).
- Do not build a Story without an approved plan, or two Stories at once.
- Do not expand scope, weaken criteria or implement unrelated work.
- Do not mark the Story done or claim acceptance; only the PO accepts, via
  `/accept-story`.
- Do not publish.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the implementation and cannot be determined from
  existing context.
