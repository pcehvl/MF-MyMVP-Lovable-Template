---
name: accept-story
description: Use when the PO explicitly invokes /accept-story US-XXX to accept or reject a built Story on the preview, one Acceptance Criterion at a time. Use only when explicitly invoked with /accept-story.
---

# Accept Story

Walk the PO through every Acceptance Criterion on the preview, record the
result in `acceptance.md`, and close the Story only on explicit PO
acceptance.

## 1. Preconditions

- `spec.md` has `status: ready` and `plan.md` has `status: approved`
- the Story has been built (a `/build-story` report exists in this project's
  history, or the PO says so)

If a precondition fails, stop with a one-line explanation.

## 2. Before asking the PO

Prepare in one message:

- the list of files and areas changed since the plan was approved
- a warning if anything under `product/**` was edited during building
- the security scan status if visible (critical findings block acceptance)
- the earlier `done` Stories whose areas this change touched, with an offer to
  re-check their ACs on the preview (there are no automated tests, so this is
  the regression safety net)

## 3. One ACn at a time

For each ACn in order, in its own message:

- name the ACn by ID and short title, and say where to look in the preview
- ask the PO one question: PASS or FAIL, with what they observed

Do not move to the next ACn before the PO has answered.

## 4. Write `acceptance.md`

Write `product/stories/<US-XXX-slug>/acceptance.md`:

```markdown
---
story: US-XXX
spec_revision: <as in plan.md>
date: <YYYY-MM-DD>
lovable_version: <version or commit if visible, else "not visible">
result: accepted | rejected
---

# Acceptance — US-XXX

| ACn | Result | PO observation |
|---|---|---|
| AC1 | PASS | … |

## Security scan

<status as seen, or "not visible">

## Changed since plan

<files and areas; flag any product/** edits>

## Regression checks

<earlier Stories re-checked, or "none requested">

## Limitations

<known limitations, or "none">
```

If an `acceptance.md` already exists from an earlier round, add the new round
below the old one instead of deleting it.

## 5. Outcome

- **All PASS and the PO explicitly accepts:** set `status: done` in the
  Story's `spec.md` frontmatter (change nothing else in that file) and confirm
  in one line.
- **Any FAIL:** do not change `spec.md`. Write a ready-to-use fix prompt that
  cites each failing ACn by ID and the PO's observation, and tell the PO to
  run it, then `/accept-story US-XXX` again.
- **All PASS but the PO does not accept:** record the reason and ask what
  should happen next.

## 6. Must not

- Do not record PASS on the PO's behalf, or accept the Story yourself.
- Do not change app code, the database or `plan.md` while accepting.
- Do not edit `spec.md` except to set `status: done` after explicit
  acceptance.
- Do not accept while the security scan shows critical findings.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the result and cannot be determined from existing
  context.
