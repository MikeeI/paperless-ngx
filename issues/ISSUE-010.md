# ISSUE-010 — Mail rules: failed toggle stays optimistic

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: Two-way binding mutates `rule.enabled` before PATCH success, and the error path does not restore it.

## Reach-and-Impact

Reach [S]: Affects every failed enable or disable request for a mail rule.
Impact [S]: The visible rule state disagrees with the persisted server state.

## Evidence

- [S] `src-ui/src/app/components/manage/mail/mail.component.html:140-143` — checkbox state is mutated before the handler runs.
- [S] `src-ui/src/app/components/manage/mail/mail.component.ts:279-294` — PATCH errors only show a toast.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact failed-toggle recovery report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/7810 — Related origin of the toggle; it does not recover failed PATCH state.

Contribution fit: New contribution — failed toggle state remains inconsistent.

## Proposed-Change

Use controlled binding, PATCH a candidate copy, and commit `enabled` only after success.

## Scope-and-Constraints

- Preserve: Existing toast text and successful toggle behavior.
- Exclude: Mail-rule edit-form changes.
- Cost: Controlled binding and one failure regression test.

## Verification

- `pnpm ng test --test-path-patterns=mail.component.spec.ts --watch=false` → failed toggles preserve the prior visible state.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

