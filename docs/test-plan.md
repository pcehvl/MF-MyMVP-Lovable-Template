# Test plan

Run these before using the harness with students. Use a throwaway Lovable
project and a copy of this repository. Record the result (PASS/FAIL and
notes) in the table at the end.

## T1 — OpenCode reaches Lovable (OAuth)

1. Open the repository in OpenCode and run `/mcps`.
2. Select `lovable` and sign in.

**Expect:** `lovable` is connected and the agent can list your workspaces
and projects. Lovable's MCP server supports only OAuth and does not list
OpenCode among documented clients, so this is the first thing to confirm.

## T2 — Permissions hold

1. Ask the agent to publish the Lovable project, and to run a database
   query.
2. Ask it to edit a file outside `product/**` and `docs/design-thinking/**`.

**Expect:** all refused (denied by `opencode.json` and the agent's
permissions). `send_message` asks before it runs.

## T3 — Handover arrives verbatim

1. Create a small ready Story with two ACn (`revision: 1`).
2. `/handover US-001`.

**Expect:** Lovable runs `receive-story`, writes `product/product.md` and the
spec unchanged, and the read-back reports "received". Note which `ref` value
worked for `read_file` (`HEAD` or `main`).

## T4 — The handover is visible in the Lovable chat

Open the project in Lovable after T3.

**Expect:** the handover message and Lovable's reply appear in the project
chat, so the student can continue there. If they do not appear, the student
must rely on the files and the report in OpenCode.

## T5 — Execution loop

`plan US-001` → approve → save → `/build-story US-001` → `/accept-story
US-001` with one FAIL, fix, then all PASS, plus one new wish.

**Expect:** `plan.md` pinned to rev 1; build report with ACn table;
`acceptance.md` with two rounds, `result: accepted` and the new wish.

## T6 — Return and revision

1. `/return US-001` in OpenCode.
2. Decide to change the Story (bump to rev 2) and `/handover US-001` again.
3. In Lovable, try `/build-story US-001` without re-planning.

**Expect:** the return is saved and summarised; `receive-story` warns that
the plan was for rev 1; `build-story` refuses until a new plan is approved.

## T7 — Scope guard

In Lovable, ask for a feature that is not in the spec.

**Expect:** refused, with the offer to note it under "New wishes".

## T8 — Credits

Note the credits used for T3–T6.

**Expect:** a rough cost per Story to share with students.

## T9 — Paste fallback

Disconnect `lovable` in OpenCode and run `/handover US-001`.

**Expect:** the handover block is printed; pasting it into Lovable works like
T3.

## Results

| Test | Result | Notes |
|---|---|---|
| T1 | | |
| T2 | | |
| T3 | | |
| T4 | | |
| T5 | | |
| T6 | | |
| T7 | | |
| T8 | | |
| T9 | | |
