---
name: receive-story
description: Use when a message contains a HANDOVER block (=== HANDOVER US-XXX rev N ===) from the product workspace, or the PO types /receive-story. Saves the handed-over product.md and spec.md as read-only copies.
---

# Receive Story

Store a handover from the product workspace exactly as sent. This is the only
way `product/product.md` and a `spec.md` may change in this project.

## 1. The handover format

```text
=== HANDOVER US-XXX rev N ===
--- FILE product/product.md ---
<content>
--- FILE product/stories/<US-XXX-slug>/spec.md ---
<content>
=== END HANDOVER ===
```

If the block is incomplete (missing a FILE section or the END line), stop and
say what is missing. Do not guess content.

## 2. Checks before writing

- The spec's frontmatter `id` matches `US-XXX` and its `revision` matches
  `N`, and its `status` is `ready`. Otherwise stop and report the mismatch.
- If a copy of this spec already exists with a **higher** revision, stop:
  the handover is outdated.
- If the same revision already exists and is identical, report "already
  received" and stop.

## 3. Write the copies

Write each FILE section to its path **verbatim**: same text, headings, line
breaks and frontmatter. Do not summarise, translate, reformat or fix
anything. Then re-read both files and compare them with the handover; fix any
difference before reporting.

Write nothing else: no app code, no plan, no acceptance record.

## 4. Report

In one short message:

- "Received US-XXX rev N: AC1–ACn" (list the ACn IDs and short titles)
- if this replaces an older revision and a `plan.md` exists for the Story:
  "The approved plan was for rev M; plan again before building."
- if another Story is in flight: name it and say it should be accepted
  first
- next step: "Plan mode: plan US-XXX"

## 5. Must not

- Do not change app code, the database or access rules.
- Do not edit the handed-over content in any way.
- Do not write `product.md` or a `spec.md` from anything other than a
  handover block.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the result and cannot be determined from existing
  context.
