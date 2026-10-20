---
name: publish
description: Use when the PO explicitly invokes /publish to release the app. Checks the release preconditions, hands the Publish click to the PO and logs the live check. Use only when explicitly invoked with /publish.
---

# Publish

Publishing is a PO-authorised act. You check the preconditions and guide the
PO; the PO clicks Publish in Lovable.

## 1. Preconditions

Check and report each one as OK or BLOCKED in a single message:

- every Story being released has `status: done` in its `spec.md`
- no Story is in flight with unfinished work in the app (if one is, say that
  its partial work would be published too)
- the security scan shows no critical findings (if you cannot see the scan
  result, say so and ask the PO to open it; never assume it passed)
- no secrets or privileged credentials in client code
- the PO has explicitly authorised this release

If anything is BLOCKED, stop and say what is needed.

## 2. Hand over the click

When all preconditions are OK, tell the PO to click **Publish** in Lovable
and to tell you the live URL when it is done. Do not claim the app is
published before the PO confirms it.

## 3. After publishing

With the PO, one question at a time:

- open the live URL and confirm it loads
- walk through the critical user journey on the live app
- confirm data and access rules behave as on the preview

Append to each released Story's `acceptance.md`:

```markdown
## Release <YYYY-MM-DD>

- live URL: <url>
- Lovable version: <version or commit if visible>
- live check: <PASS/FAIL per step, with PO observations>
```

## 4. Rollback

If the live check fails, the rollback path is Lovable's version history:
restore the last known-good version and publish again, on the PO's decision.
Record the rollback in the same Release section.

## 5. Must not

- Do not publish, or tell the PO to publish, while any precondition is
  BLOCKED.
- Do not change app code, the database, `spec.md` or `plan.md` while
  publishing.
- Do not mark anything as released before the PO confirms the live URL.
- Ask the PO at most ONE question per message, and only when the answer
  materially affects the release and cannot be determined from existing
  context.
