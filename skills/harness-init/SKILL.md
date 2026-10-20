---
name: harness-init
description: Use when the PO explicitly invokes /harness-init in a new Lovable project to install the MyMVP harness files. Use only when explicitly invoked with /harness-init.
---

# Harness Init

Install the MyMVP harness files into this project by copying the bundled
`templates/` folder. Run once per project, in Build mode.

## 1. Files to create

Copy each bundled file to the same path at the project root:

| Bundled file | Project path |
|---|---|
| `templates/AGENTS.md` | `AGENTS.md` |
| `templates/product/product.md` | `product/product.md` |
| `templates/product/stories/.gitkeep` | `product/stories/.gitkeep` |
| `templates/docs/design-thinking/.gitkeep` | `docs/design-thinking/.gitkeep` |

## 2. Procedure

1. Check whether any target path already exists. If one does, stop, list the
   existing files, and ask the PO whether to skip them or overwrite them. Do
   nothing else until the PO answers.
2. Copy every file verbatim: same text, headings, comments and line breaks.
   Do not summarise, translate, reformat or "improve" them.
3. Re-read each written file and compare it with its bundled source. If any
   file differs, fix it and say so in the report.
4. Report the files created in a short list, then: "Next: describe your
   product in Plan mode; the product-assistant skill takes it from there."

If you are in Plan mode and cannot write files, say so in one line and ask
the PO to switch to Build mode and run `/harness-init` again.

## 3. Must not

- Do not touch app code, the database, styling or configuration.
- Do not fill in `product/product.md`; it stays a template until the PO
  frames the product.
- Do not create Stories, plans or any other file than those listed in §1.
- Do not overwrite an existing file without the PO's explicit answer.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the setup and cannot be determined from existing context.
