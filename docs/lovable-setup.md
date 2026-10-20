# Lovable setup and daily use

Everything below happens in the Lovable UI. You do not need a terminal.

## 1. Workspace setup (once per workspace)

You must be a workspace owner or admin to manage skills.

1. **Import the skills.** Workspace settings → Skills → import. Import each
   folder under `skills/` as its own skill. Make a ZIP of one folder at a
   time (the folder itself, with `SKILL.md` at its top level). For
   `harness-init`, include its `templates/` folder in the same ZIP.
   If a GitHub Release with ready-made ZIPs exists, use those instead.
2. **Set auto-use.** Turn automatic use **OFF** for the gate skills
   `harness-init`, `build-story`, `accept-story` and `publish`, so they run
   only when you type `/<name>`. Leave it ON for `product-assistant`,
   `product-management`, `feedback-management`, `ui-design` and `plan-story`.
3. **Add workspace knowledge.** Workspace settings → Knowledge: paste the
   whole of `lovable/workspace-knowledge.md`.

## 2. New product (once per project)

1. Create a new Lovable project.
2. Optional: connect GitHub. Lovable creates a new repository for the
   project; it cannot use this harness repository.
3. Switch to Build mode and type `/harness-init`. It creates `AGENTS.md`,
   `product/product.md`, `product/stories/`, `docs/ui-guidelines.md` and
   `docs/design-thinking/`, and reports what it created.

## 3. Daily loop

### Product loop

1. In Plan mode, describe the product, users and problems freely. The
   product-assistant skill frames the product with you, one question at a
   time.
2. Switch to Build mode and say "save". It writes `product/product.md` or a
   Story's `spec.md`.
3. When a Story is clear, confirm it is ready. Only then is it set to
   `status: ready`.

### Execution loop

1. **Plan.** In Plan mode, say "plan US-XXX". You get a plan of at most 200
   words. Adjust it or approve it.
2. **Save the plan.** In Build mode, say "save the plan". It writes
   `plan.md` with `status: approved`.
3. **Build.** In Build mode, type `/build-story US-XXX`. Lovable builds the
   Story, fixes its own errors and reports a table of how each ACn is met.
4. **Check.** Look at the preview yourself.
5. **Accept.** Type `/accept-story US-XXX`. You answer PASS or FAIL for one
   ACn at a time. All PASS plus your explicit acceptance sets the Story to
   `done`. A FAIL gives you a fix prompt; run it and accept again.
6. **Publish.** Type `/publish`. It checks the preconditions; you click
   Publish; together you check the live app.
7. **Feedback.** Bring feedback, bug reports or test observations, or type
   `/feedback-management`. You decide what happens: update a Story, create a
   Story, update the product context, or no action.

Small, low-risk changes (styling, copy, a local bug fix) can be prompted
directly without a Story.

### Branches (optional)

The default is to work on `main` and roll back with Lovable's version
history. If you want a branch per Story, create `feature/US-XXX-slug` in
Lovable's branch picker before planning, and merge it after acceptance. A new
branch forks from the active branch. Your acceptance on the preview is the
gate, not a pull request.

## 4. Troubleshooting

| Symptom | What to do |
|---|---|
| A gate skill started on its own | Check that its auto-use is OFF. |
| "Cannot write files" in Plan mode | Switch to Build mode and repeat "save". |
| `/harness-init` stopped | A file already exists; answer skip or overwrite. |
| A copied template file looks changed | Ask Lovable to compare it with the bundled template and restore it. |
| `/build-story` refuses to start | The spec is not `ready`, the plan is not approved, or another Story is in flight. |
| Publish is blocked | Resolve the critical findings in the security scan first. |
| Database changes pushed from Git have no effect | Expected: only Lovable runs migrations. Ask Lovable to make the change. |
