# ISSUE-005 — Document selection: deleted IDs remain selected

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: The external-delete websocket handler reloads documents without reconciling explicit selection IDs.

## Reach-and-Impact

Reach [S]: Affects selected documents deleted by another session or external actor.
Impact [S]: Subsequent bulk requests include missing IDs and fail serializer validation.

## Evidence

- [S] `src-ui/src/app/components/document-list/document-list.component.ts:265-275` — delete events only reload the list.
- [S] `src/documents/serialisers.py:1616-1625` — missing IDs reject the complete bulk request.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact external-delete reconciliation fix was found.

- https://github.com/paperless-ngx/paperless-ngx/issues/8994 — Related; internal merge/delete refresh had a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/issues/11469 — Related and rejected; reported a different selected-deletion workflow.

Contribution fit: New contribution — external deletion still leaves stale explicit IDs.

## Proposed-Change

Reconcile explicit selection against existing filtered IDs after delete events.

## Scope-and-Constraints

- Preserve: Valid cross-page selections and all-selected mode.
- Exclude: Backend tolerance for missing document IDs.
- Cost: One reconciliation request after delete events.

## Verification

- `pnpm ng test --test-path-patterns=document-list.component.spec.ts --watch=false` → delete events trigger selection reconciliation.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

