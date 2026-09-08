# ISSUE-008 — Tasks: stale requests overwrite current page

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

Root-Cause [S]: Page and status-count reloads subscribe independently, so older HTTP responses can replace newer task state.

## Reach-and-Impact

Reach [S]: Affects rapid task filter, page, sort, and auto-refresh changes.
Impact [S]: The table or counts can display data for a request that is no longer current.

## Evidence

- [S] `src-ui/src/app/components/admin/tasks/tasks.component.ts:210-266` — direct subscriptions have component teardown but no latest-request cancellation.
- [S] `src-ui/src/app/services/tasks.service.ts:111-158` — list and count methods expose ordinary HTTP observables.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact stale-response report was found.

- https://github.com/paperless-ngx/paperless-ngx/issues/12951 — Related; server filtering had a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/pull/12956 — Related fix; it does not establish latest-request ownership.

Contribution fit: New contribution — task refresh requests remain unordered.

## Proposed-Change

Cancel the previous page and count subscriptions before starting replacements and on teardown.

## Scope-and-Constraints

- Preserve: Existing auto-refresh cadence and task query semantics.
- Exclude: Tasks-service cache redesign.
- Cost: Two explicit component-owned subscriptions and ordering tests.

## Verification

- `pnpm ng test --test-path-patterns=tasks.component.spec.ts --watch=false` → only the newest page and count responses update state.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

