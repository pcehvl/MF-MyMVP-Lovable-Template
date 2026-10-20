---
name: harness-init
description: Use when the PO explicitly invokes /harness-init in a new Lovable project to install the MyMVP execution-loop files. Use only when explicitly invoked with /harness-init.
---

# Harness Init

Prepare this Lovable project to receive Story handovers from the product
workspace, by copying the bundled `templates/` folder. Run once per project,
in Build mode.

## 1. Files to create

Copy each bundled file to the same path at the project root:

| Bundled file | Project path |
|---|---|
| `templates/AGENTS.md` | `AGENTS.md` |
| `templates/product/stories/.gitkeep` | `product/stories/.gitkeep` |

`product/product.md` and the Story specs are **not** created here; they
arrive with the first handover.

## 2. Procedure

1. Check whether any target path already exists. If one does, stop, list the
   existing files, and ask the PO whether to skip them or overwrite them. Do
   nothing else until the PO answers.
2. Copy every file verbatim: same text, headings, comments and line breaks.
   Do not summarise, translate, reformat or "improve" them.
3. Re-read each written file and compare it with its bundled source. If any
   file differs, fix it and say so in the report.
4. Report the files created in a short list, then: "Next: hand a ready Story
   over from the product workspace (`/handover US-XXX` in OpenCode)."

If you are in Plan mode and cannot write files, say so in one line and ask
the PO to switch to Build mode and run `/harness-init` again.

## 3. Must not

- Do not touch app code, the database, styling or configuration.
- Do not create `product/product.md`, Stories, plans or any file other than
  those listed in §1.
- Do not overwrite an existing file without the PO's explicit answer.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the setup and cannot be determined from existing context.
