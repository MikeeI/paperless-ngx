# ISSUE-011 — Workflows: reload failure leaves loading active

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

Root-Cause [S]: Workflow-list loading is cleared only in the reload success callback.

## Reach-and-Impact

Reach [S]: Affects every failed workflow-list request.
Impact [S]: Loading remains active and the list offers no local recovery feedback.

## Evidence

- [S] `src-ui/src/app/components/manage/workflows/workflows.component.ts:63-88` — reload has a success-only subscription.
- [S] https://github.com/paperless-ngx/paperless-ngx/pull/14023 — Related retrieval error handling; main-list reload remains unaffected.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact main-list reload recovery report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/14023 — Related partial fix; workflow-dialog related-object retrieval now catches errors.

Contribution fit: New contribution — main workflow reload still lacks finalization.

## Proposed-Change

Finalize list loading on every outcome and surface request errors with the existing toast service.

## Scope-and-Constraints

- Preserve: Successful list sorting and refresh behavior.
- Exclude: Workflow-dialog retrieval behavior fixed separately upstream.
- Cost: One observable finalizer and focused failure test.

## Verification

- `pnpm ng test --test-path-patterns=workflows.component.spec.ts --watch=false` → failed reload clears loading state.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

