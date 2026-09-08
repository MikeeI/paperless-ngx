# ISSUE-004 — Document selection: stale pruning response wins

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

Root-Cause [S]: `reduceSelectionToFilter` applies every asynchronous ID response without cancellation or freshness ownership.

## Reach-and-Impact

Reach [S]: Affects rapid filter changes while explicit document selection exists.
Impact [S]: An older filter response can remove IDs valid under the current filter.

## Evidence

- [S] `src-ui/src/app/services/document-list-view.service.ts:407-420,670-685` — pruning subscribes independently of reload cancellation.
- [S] https://github.com/paperless-ngx/paperless-ngx/pull/13229 — Related filtered selection-data work; no request-ordering fix.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact response-ordering report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/13229 — Related; backend selection-data performance has a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/pull/13191 — Related; select-all custom-field behavior has a different root cause.

Contribution fit: New contribution — stale pruning requests remain unowned.

## Proposed-Change

Bind pruning requests to the existing reload cancellation notifier so newer reloads supersede older pruning.

## Scope-and-Constraints

- Preserve: Explicit selection across matching filter changes.
- Exclude: Selection UX redesign.
- Cost: One lifecycle operator and race regression test.

## Verification

- `pnpm ng test --test-path-patterns=document-list-view.service.spec.ts --watch=false` → an older pruning response cannot mutate current selection.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

