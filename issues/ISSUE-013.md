# ISSUE-013 — Filter editor: loading can remain active forever

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

Root-Cause [S]: Optional metadata loads decrement completion only on success and never complete when zero requests are authorized.

## Reach-and-Impact

Reach [S]: Affects restricted users with no metadata permissions and users whose optional metadata request fails.
Impact [S]: The filter editor remains in loading state indefinitely.

## Evidence

- [S] `src-ui/src/app/components/document-list/filter-editor/filter-editor.component.ts:1226-1298` — completion accounting lacks zero-request and error paths.
- [S] https://github.com/paperless-ngx/paperless-ngx/issues/13162 — Related spinner behavior in a different saved-view widget.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact filter-editor metadata fan-in fix was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/13164 — Related spinner fix in a different widget.
- https://github.com/paperless-ngx/paperless-ngx/issues/1819 — Distinct pagination-spinner behavior.

Contribution fit: New contribution — this fan-in completion contract remains incomplete.

## Proposed-Change

Finalize each authorized metadata request, tolerate optional lookup errors, and explicitly finish the zero-request case.

## Scope-and-Constraints

- Preserve: Available filter controls and successful metadata ordering.
- Exclude: Metadata API batching.
- Cost: Shared completion finalization and focused zero/error tests.

## Verification

- `pnpm ng test --test-path-patterns=filter-editor.component.spec.ts --watch=false` → zero and failed optional loads both finish loading.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

