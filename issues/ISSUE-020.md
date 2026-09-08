# ISSUE-020 — Workflow dialog: failed delete disables retry

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: Workflow deletion disables confirmation buttons before DELETE but does not re-enable them on error.

## Reach-and-Impact

Reach [S]: Affects every failed workflow deletion request.
Impact [S]: The open confirmation dialog cannot retry or cancel without being recreated.

## Evidence

- [S] `src-ui/src/app/components/manage/workflows/workflows.component.ts:129-157` — error handling only shows a toast.
- [S] `src-ui/src/app/components/common/confirm-dialog/confirm-dialog.component.html:20-35` — every footer control uses `buttonsEnabled`.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact failed-delete recovery report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/13672 — Related signal-wiring fix; it does not restore controls after error.

Contribution fit: New contribution — workflow delete retries remain blocked.

## Proposed-Change

Restore confirmation buttons in the workflow delete error callback.

## Scope-and-Constraints

- Preserve: Button locking while active and modal dismissal on success.
- Exclude: Shared confirmation-dialog redesign.
- Cost: One error-path assignment and focused test.

## Verification

- `pnpm ng test --test-path-patterns=workflows.component.spec.ts --watch=false` → failed deletion re-enables controls.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

