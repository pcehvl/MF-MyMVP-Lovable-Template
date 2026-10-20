# Setup and daily use

## 1. Teacher: Lovable workspace (once)

You must be a Lovable workspace owner or admin.

1. **Import the skills.** Workspace settings → Skills → import. Import each
   folder under `lovable/skills/` as its own skill (one ZIP per folder, with
   `SKILL.md` at its top level). For `harness-init`, include its
   `templates/` folder in the same ZIP.
2. **Set auto-use.** Turn automatic use **OFF** for `harness-init`,
   `build-story`, `accept-story` and `publish`. Leave it **ON** for
   `receive-story` and `plan-story`.
3. **Add workspace knowledge.** Workspace settings → Knowledge: paste the
   whole of `lovable/workspace-knowledge.md`.
4. **MCP access.** Third-party MCP clients are allowed by default on Free,
   Pro and Business; on Enterprise an admin must enable them.

Run the tests in [`test-plan.md`](test-plan.md) before the first class.

## 2. Student: new product (once)

1. **Product workspace.** On GitHub, open this template and click **Use this
   template** to create your own repository. Clone it (GitHub Desktop is
   fine) and open the folder in OpenCode. The `product-assistant` agent
   starts by default.
2. **Sign in to Lovable from OpenCode.** Run `/mcps`, select `lovable` and
   finish the sign-in in the browser. You do this once.
3. **Lovable project.** In Lovable, create a new project. Switch to Build
   mode and type `/harness-init`.
4. **Link them.** The first `/handover` asks which Lovable project belongs
   to this product and remembers it in `product/lovable-project.md`.

## 3. Product loop (OpenCode)

1. **Evidence.** Give the agent your interviews, notes or observations. It
   writes one evidence card per source in `docs/design-thinking/`
   (`E-001-…`). Keep large raw files (recordings, PDFs) where they are and
   give the link.
2. **Framing.** Work through `product/product.md` with the agent, one
   question at a time.
3. **Stories.** Create Stories with Acceptance Criteria
   (`- **AC1 — Short Title.** …`). When a Story is clear and you confirm it,
   it becomes `ready` with `revision: 1`.
4. **Hand over.** `/handover US-001`. The agent sends `product.md` and the
   spec to Lovable, then reads them back to check they arrived unchanged.

## 4. Execution loop (Lovable)

1. **Plan.** Plan mode: `plan US-001`. Adjust or approve the plan.
2. **Save the plan.** Build mode: "save the plan".
3. **Build.** `/build-story US-001`. Lovable builds, fixes its own errors and
   shows how each ACn is met.
4. **Check** the preview yourself.
5. **Accept.** `/accept-story US-001`: PASS or FAIL per ACn, one at a time,
   then any new wishes. A FAIL gives you a fix prompt; run it and accept
   again.
6. **Publish** when you want to release: `/publish`, then click Publish.

Asking Lovable for something that is not in the spec is refused on purpose:
take it back to the product workspace.

## 5. Back to the product loop

`/return US-001` in OpenCode fetches `acceptance.md`.

- Accepted against the current revision → confirm, and the Story is `done`.
- New wishes or observations → the agent analyses them with you; you decide:
  change a `ready` Story (new revision → `/handover` again), create a new
  Story, update the product context, or do nothing.

## 6. Without MCP (fallback)

If signing in to Lovable from OpenCode does not work, `/handover` prints the
handover block: paste it into the Lovable chat in Build mode. For a return,
copy `acceptance.md` from Lovable's Code view and paste it into OpenCode.

## 7. Troubleshooting

| Symptom | What to do |
|---|---|
| `/handover` says the Lovable tools are unavailable | Run `/mcps` and sign in to `lovable`. |
| Read-back after handover differs | Let the agent resend once; if it still differs, use the paste fallback and tell your teacher. |
| `receive-story` did not start in Lovable | Type `/receive-story` and paste the handover block again. |
| `/build-story` refuses | The plan is not approved, or it was made for an older revision: plan again. |
| A gate skill started on its own | Its auto-use must be OFF (teacher). |
| Publish is blocked | Resolve the critical findings in the security scan first. |
