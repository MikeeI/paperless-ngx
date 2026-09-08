# ISSUE-009 — Bulk editor: failed download remains pending

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

Root-Cause [S]: `awaitingDownload` is cleared only by the bulk-download success callback.

## Reach-and-Impact

Reach [S]: Affects every failed bulk-download HTTP request.
Impact [S]: The spinner and disabled download control remain latched until the component is recreated.

## Evidence

- [S] `src-ui/src/app/components/document-list/bulk-editor/bulk-editor.component.ts:461-478` — the request has no error cleanup or finalizer.
- [S] `src-ui/src/app/components/document-list/bulk-editor/bulk-editor.component.html:288-309` — pending state disables the action.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact failed-download state-recovery report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/8631 — Related; backend permission validation has a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/pull/13103 — Related; selected-version download behavior has a different root cause.

Contribution fit: New contribution — error state is not finalized.

## Proposed-Change

Finalize pending state for both success and error, and report the failure through the existing toast service.

## Scope-and-Constraints

- Preserve: Successful download filename and browser-save behavior.
- Exclude: Download transport redesign.
- Cost: One observable finalizer and failure regression test.

## Verification

- `pnpm ng test --test-path-patterns=bulk-editor.component.spec.ts --watch=false` → failed download clears pending state.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

