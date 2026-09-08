# ISSUE-017 — Document list: delete subscription outlives component

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

Root-Cause [S]: The document-delete websocket subscription omits the component teardown notifier used by sibling subscriptions.

## Reach-and-Impact

Reach [S]: Affects every destroyed document-list component retained by the long-lived websocket service.
Impact [S]: Later delete events invoke reloads on obsolete list instances and retain component state.

## Evidence

- [S] `src-ui/src/app/components/document-list/document-list.component.ts:265-275,292-301` — delete subscription is unbounded despite an existing destroy notifier.
- [S] `src-ui/src/app/services/websocket-status.service.ts:335-352` — the delete subject belongs to a longer-lived service.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact teardown-leak report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/8996 — Related; introduced delete refresh for a stale-list defect without lifecycle teardown.
- https://github.com/paperless-ngx/paperless-ngx/issues/8994 — Related source behavior; not a teardown report.

Contribution fit: New contribution — the subscription remains unowned at component destruction.

## Proposed-Change

Bind the delete subscription to the existing `unsubscribeNotifier` lifecycle.

## Scope-and-Constraints

- Preserve: Live delete-triggered reload behavior while the component is mounted.
- Exclude: Websocket-service lifecycle changes.
- Cost: One lifecycle operator and teardown regression test.

## Verification

- `pnpm ng test --test-path-patterns=document-list.component.spec.ts --watch=false` → delete events after destruction do not reload the list.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

