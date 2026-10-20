---
name: lovable-bridge
description: Use when the PO wants to hand a ready Story over to Lovable (/handover US-XXX) or fetch a Story's result back from Lovable (/return US-XXX). The only way this product workspace talks to Lovable.
---

# Lovable Bridge

The product loop runs here in OpenCode; the execution loop runs in Lovable.
Only two things cross between them:

```text
HANDOVER  OpenCode → Lovable   product.md + one ready spec.md
RETURN    Lovable → OpenCode   that Story's acceptance.md
```

Everything else (planning, building, fixing, acceptance, publishing) happens
in Lovable, driven by the PO there.

## 1. Allowed Lovable tools

Use only these MCP tools: `lovable_list_workspaces`, `lovable_list_projects`,
`lovable_get_project`, `lovable_send_message`, `lovable_get_message`,
`lovable_list_messages`, `lovable_list_files`, `lovable_read_file`.
`send_message` is used only to deliver a handover. Never send any other
instruction to Lovable.

If the Lovable tools are unavailable (not signed in), say so in one line,
tell the PO to run `/mcps` and sign in to `lovable`, and offer the manual
fallback (§5).

## 2. The linked Lovable project

The link is stored in `product/lovable-project.md`:

```markdown
---
workspace_id: <id>
project_id: <id>
project_name: <name>
---
```

If the file is missing, list the PO's workspaces and projects, ask the PO
which project belongs to this product (one question), and write the file.

## 3. Handover (`/handover US-XXX`)

1. If git is set up, `git pull` first.
2. Preconditions — stop with a one-line reason if any fails:
   - `product/stories/<US-XXX-slug>/spec.md` exists, `status: ready`, and has
     a `revision`
   - its Acceptance Criteria use the `ACn` format
3. Build the handover message exactly like this, with the files' full
   current contents:

   ```text
   /receive-story
   === HANDOVER US-XXX rev N ===
   --- FILE product/product.md ---
   <full content of product/product.md>
   --- FILE product/stories/<US-XXX-slug>/spec.md ---
   <full content of spec.md>
   === END HANDOVER ===
   ```

4. Show the PO one line ("Hand over US-XXX rev N to <project_name>?") and send
   it with `lovable_send_message` (`project_id`, `message`, no `plan_mode`).
   Sending costs Lovable credits.
5. Check the result: read both files back with `lovable_read_file`
   (`project_id`, `path`, `ref`; try `ref: "HEAD"`, then `"main"`). Compare
   them with the local files, ignoring only trailing whitespace.
   - identical → report "US-XXX rev N received by Lovable" and the next step:
     "In Lovable: plan US-XXX".
   - different or missing → report what differs, and offer to send the
     handover again once. Do not loop.
6. If the send timed out, poll with `lovable_get_message` before resending.

## 4. Return (`/return US-XXX`)

1. If git is set up, `git pull` first.
2. Read `product/stories/<US-XXX-slug>/acceptance.md` from Lovable with
   `lovable_read_file`. If it does not exist, say the Story has not been
   accepted in Lovable yet and stop.
3. Save it verbatim to the same path here. If a copy already exists, replace
   it only if the Lovable version is newer (more rounds or a later date).
4. Summarise in a few lines: result (accepted/rejected), the `spec_revision`
   it was accepted against, FAIL rows, new wishes, limitations.
5. Next step, by case:
   - accepted against the current `revision` → ask the PO to confirm `done`;
     on yes, set `status: done` in `spec.md`.
   - accepted against an older revision → the current revision has not been
     built; say so and offer `/handover US-XXX`.
   - new wishes, observations or FAIL notes → hand them to the
     `feedback-management` skill. Each needs a PO decision.

## 5. Manual fallback (no MCP)

- Handover: print the §3 block in a code block and tell the PO to paste it
  into the Lovable chat in Build mode.
- Return: ask the PO to open `acceptance.md` in Lovable (Code view), copy it
  and paste it here, then continue from §4 step 3.

## 6. Must not

- Do not send Lovable anything except a handover block.
- Do not use `deploy_project`, `query_database`, `create_project` or any tool
  not listed in §1, even if available.
- Do not hand over a Story that is not `ready`, or a stale copy of a file.
- Do not edit a returned `acceptance.md` after saving it.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the work and cannot be determined from existing context.
