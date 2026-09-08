# ISSUE-014 — Bulk editor: action permission gates are inconsistent

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: Medium
Finding-Category: UI
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: Bulk-action buttons use different global and object-permission predicates, including page-local checks for cross-page selection.

## Reach-and-Impact

Reach [S]: Affects restricted users and all-selected operations that extend beyond the loaded page.
Impact [S]: Controls can advertise operations that the backend rejects, while equivalent actions use inconsistent gates.

## Evidence

- [S] `src-ui/src/app/components/document-list/bulk-editor/bulk-editor.component.html:78-309` — action buttons apply inconsistent and duplicated conditions.
- [S] `src-ui/src/app/components/document-list/bulk-editor/bulk-editor.component.ts:127-144` — ownership predicates inspect only current-page documents.
- [S] `src/documents/views.py:2849-2936` — backend operations require action-specific model and object permissions.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact current all-selected or page-local UI-gate report was found.

- https://github.com/paperless-ngx/paperless-ngx/discussions/8467 — Related restricted-user bulk-action report.
- https://github.com/paperless-ngx/paperless-ngx/pull/8469 — Related historical frontend global-permission fix.
- https://github.com/paperless-ngx/paperless-ngx/pull/8631 — Related backend bulk-download permission fix.

Contribution fit: New contribution — current UI gates still diverge from backend requirements.

## Proposed-Change

Centralize action-specific global gates, remove page-local claims for all-selected operations, and align merge/delete affordances with backend requirements.

## Scope-and-Constraints

- Preserve: Backend authority and object-level rejection for selections the client cannot fully inspect.
- Exclude: New permission endpoints or speculative client-side authorization.
- Cost: Focused predicate cleanup and permission-contract tests.

## Verification

- `pnpm ng test --test-path-patterns=bulk-editor.component.spec.ts --watch=false` → each action reflects its backend model-permission contract.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

