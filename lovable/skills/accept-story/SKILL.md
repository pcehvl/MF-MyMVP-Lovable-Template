---
name: accept-story
description: Use when the PO explicitly invokes /accept-story US-XXX to accept or reject a built Story on the preview, one Acceptance Criterion at a time. Use only when explicitly invoked with /accept-story.
---

# Accept Story

Walk the PO through every Acceptance Criterion on the preview and record the
result in `acceptance.md`. The product workspace fetches this file as the
Story's **return**, so it must be complete and self-explanatory.

## 1. Preconditions

- `spec.md` has `status: ready`, and `plan.md` is `approved` for the same
  revision
- the Story has been built (a `/build-story` report exists, or the PO says
  so)

If a precondition fails, stop with a one-line explanation.

## 2. Before asking the PO

Prepare in one message:

- the files and areas changed since the plan was approved
- a warning if `product.md` or any `spec.md` changed outside `receive-story`
- the security scan status if visible (critical findings block acceptance)
- the earlier accepted Stories whose areas this change touched, with an offer
  to re-check their ACs on the preview (there are no automated tests, so this
  is the regression safety net)

## 3. One ACn at a time

For each ACn in order, in its own message:

- name the ACn by ID and short title, and say where to look in the preview
- ask the PO one question: PASS or FAIL, with what they observed

Do not move to the next ACn before the PO has answered. At the end, ask once:
"Any new wishes or observations for the product workspace?"

## 4. Write `acceptance.md`

Write `product/stories/<US-XXX-slug>/acceptance.md`:

```markdown
---
story: US-XXX
spec_revision: <N>
date: <YYYY-MM-DD>
lovable_version: <version or commit if visible, else "not visible">
result: accepted | rejected
---

# Acceptance — US-XXX rev N

| ACn | Result | PO observation |
|---|---|---|
| AC1 | PASS | … |

## New wishes

<ideas, changed requirements or observations outside the spec, in the PO's
words; or "none">

## Security scan

<status as seen, or "not visible">

## Changed since plan

<files and areas; flag any product.md or spec.md changes>

## Regression checks

<earlier Stories re-checked, or "none requested">

## Limitations

<known limitations, or "none">
```

If an `acceptance.md` already exists from an earlier round, add the new round
below the old one instead of deleting it, and update the frontmatter to the
latest round.

## 5. Outcome

- **All PASS and the PO explicitly accepts:** set `result: accepted` and
  tell the PO: "Run `/return US-XXX` in the product workspace."
- **Any FAIL:** set `result: rejected`, write a ready-to-use fix prompt that
  cites each failing ACn and the PO's observation, and tell the PO to run it,
  then `/accept-story US-XXX` again.
- **All PASS but the PO does not accept:** set `result: rejected`, record the
  reason, and say that a spec change in the product workspace is probably
  needed.

## 6. Must not

- Do not record PASS on the PO's behalf, or accept the Story yourself.
- Do not change app code, the database, `plan.md`, `spec.md` or
  `product.md` while accepting.
- Do not build anything from "New wishes"; they only travel back.
- Do not accept while the security scan shows critical findings.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the result and cannot be determined from existing
  context.
