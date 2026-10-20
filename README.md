# MyMVP for Lovable — spec-driven harness

OpenCode runs the product loop; Lovable runs the execution loop. Specs go
over with `/handover`, results come back with `/return`.

| Path | Used by |
|---|---|
| `AGENTS.md`, `opencode.json`, `.opencode/`, `product/`, `docs/design-thinking/` | OpenCode |
| `lovable/skills/`, `lovable/workspace-knowledge.md` | Lovable workspace |

## Run

1. **Lovable (once per workspace):** import each folder in `lovable/skills/`
   as a skill (ZIPs on the latest release). Turn auto-use off for
   `harness-init`, `build-story`, `accept-story` and `publish`. Paste
   `lovable/workspace-knowledge.md` into workspace knowledge.
2. **Product repository:** **Use this template**, clone it and open it in
   OpenCode. Run `/mcps` and sign in to `lovable`.
3. **Lovable project:** create a project and run `/harness-init` in Build
   mode.
4. **Loop:** `/handover US-XXX` in OpenCode → plan, `/build-story`,
   `/accept-story`, `/publish` in Lovable → `/return US-XXX` in OpenCode.
