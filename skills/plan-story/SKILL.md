---
name: plan-story
description: Use when the PO asks to plan a ready User Story (for example "plan US-003"), or to save or revise a Story's implementation plan. Produces a concise plan for PO approval and records the approved plan in plan.md.
---

# Plan Story

Plan one `ready` Story so Lovable can build it without repeating the
analysis. The plan exists to reduce building time and credits, not to produce
documentation.

## 1. Preconditions

Check before planning, and stop with a one-line explanation if any fails:

- `product/stories/<US-XXX-slug>/spec.md` exists and has `status: ready`
- no other Story is in flight (another Story with an approved `plan.md`
  whose `spec.md` is not yet `done`); if one is, name it and ask the PO
  whether to finish it first
- the spec's Acceptance Criteria use the `ACn` format

## 2. Inspect first

Read the spec, `product/product.md`, and the parts of the existing app,
database schema and access rules the Story touches.
Plan from what exists, not from what you assume exists. If the requirement
is unclear or blocking, raise it instead of planning around it.

## 3. The plan (in Plan mode)

Present the plan in your reply, within 200 words, containing exactly:

1. Approach
2. Existing components/patterns to reuse
3. Files/areas to change
4. Frontend / backend / database changes (including tables and RLS policies)
5. Risks and dependencies
6. How each ACn will be self-checked on the preview (one line per ACn)

Reference Acceptance Criteria by `ACn` ID only; never restate criterion text.
Prefer the smallest complete vertical slice. Avoid speculative architecture,
extra layers and infrastructure no current criterion justifies.

End by asking the PO to approve or adjust the plan.

## 4. Saving the approved plan (in Build mode)

Only after the PO explicitly approves, write
`product/stories/<US-XXX-slug>/plan.md`:

```yaml
---
story: US-XXX
spec_revision: <date of the spec's last change, or a short content hash>
status: approved
approver: <PO name as given>
date: <YYYY-MM-DD>
---
```

followed by the six plan sections. If you cannot write files in the current
mode, show the final content and ask the PO to switch to Build mode and say
"save the plan".

An approved plan is not edited in place. If the spec or the approach changes
before building is finished, propose a revised plan, get approval again and
then overwrite `plan.md` with a new `date` and `spec_revision`.

## 5. Must not

- Do not build anything, change app code, the database or access rules.
- Do not edit `spec.md`, `acceptance.md` or `product/product.md`.
- Do not plan a Story that is not `ready`, and never plan two Stories at once.
- Do not redefine requirements, weaken Acceptance Criteria or expand scope;
  route requirement problems back to the product loop.
- Do not write `status: approved` without the PO's explicit approval.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the plan and cannot be determined from existing context.
