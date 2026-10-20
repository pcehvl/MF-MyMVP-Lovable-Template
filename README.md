# MyMVP for Lovable

A product-owner harness for building MVPs with Lovable. It keeps the MyMVP
product method (product framing, backlog, Story IDs, `ACn` Acceptance
Criteria and human gates) and leaves all building, database work, fixing and
publishing to Lovable. You run it entirely from the Lovable UI by prompting.

This repository is **not an app template** and contains no app code. It is a
**skill pack plus starter files** that you import into a Lovable workspace.
Lovable creates the app and its own repository.

## What is inside

| Path | What it is |
|---|---|
| `skills/` | Nine Lovable skills, one folder each with a `SKILL.md` |
| `skills/harness-init/templates/` | Starter files that `/harness-init` copies into each new project |
| `lovable/workspace-knowledge.md` | Universal rules to paste into workspace knowledge |
| `docs/lovable-setup.md` | Step-by-step setup, daily loop and troubleshooting |
| `docs/harness-design.md` | Design: roles → skills, gates, caveats |

## The skills

| Skill | Run it by | Auto-use |
|---|---|---|
| `product-assistant` | talking about the product | ON |
| `product-management` | (used by product-assistant) | ON |
| `feedback-management` | bringing feedback, or `/feedback-management` | ON |
| `ui-design` | asking for UI work | ON |
| `plan-story` | "plan US-XXX" | ON |
| `harness-init` | `/harness-init` | OFF |
| `build-story` | `/build-story US-XXX` | OFF |
| `accept-story` | `/accept-story US-XXX` | OFF |
| `publish` | `/publish` | OFF |

## First run

1. Workspace settings → Skills: import each folder under `skills/` (one ZIP per skill).
2. Turn auto-use OFF for `harness-init`, `build-story`, `accept-story` and `publish`.
3. Workspace settings → Knowledge: paste `lovable/workspace-knowledge.md`.
4. Create a new Lovable project.
5. In Build mode, type `/harness-init`.

Then describe your product in Plan mode. Full guide: `docs/lovable-setup.md`.

## The loop

```text
PRODUCT LOOP    Plan mode: talk → product-assistant → "save" → spec.md (ready after you confirm)
EXECUTION LOOP  "plan US-XXX" → approve → "save the plan" → /build-story US-XXX
                → check the preview → /accept-story US-XXX → /publish → /feedback-management
```

Derived from [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template).
