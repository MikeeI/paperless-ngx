# ISSUE-016 — Workflow editor: removal controls target wrong action

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

Root-Cause [S]: Removal actions are filtered before enumeration, so generated control paths use filtered indexes instead of original form-array indexes.

## Reach-and-Impact

Reach [S]: Affects workflows where a non-removal action precedes a removal action.
Impact [S]: Validation and control updates can address the wrong action group.

## Evidence

- [S] `src-ui/src/app/components/manage/workflows/workflow-edit-dialog/workflow-edit-dialog.component.ts:907-940` — filtering changes positional identity before control lookup.
- [S] `src-ui/src/app/components/manage/workflows/workflow-edit-dialog/workflow-edit-dialog.component.spec.ts:552-609` — current tests do not cover interleaved action kinds.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact indexed-control report or fix was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/8729 — Related workflow action handling; copy semantics are distinct.

Contribution fit: New contribution — positional identity is still lost.

## Proposed-Change

Enumerate the original action array and apply removal logic only after preserving each original index.

## Scope-and-Constraints

- Preserve: Removal-field validation and action ordering.
- Exclude: Reactive-form schema redesign.
- Cost: One loop rewrite and interleaved-action regression test.

## Verification

- `pnpm ng test --test-path-patterns=workflow-edit-dialog.component.spec.ts --watch=false` → interleaved actions update the correct removal controls.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

