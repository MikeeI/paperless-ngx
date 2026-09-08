# ISSUE-012 — Global search: workflow row bypasses permission gate

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: UI
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: Workflow button state checks permissions, but result-row activation calls the primary action without the same gate.

## Reach-and-Impact

Reach [S]: Affects users who can view global search but cannot open workflows.
Impact [S]: Mouse or keyboard row activation attempts forbidden workflow navigation despite the disabled button affordance.

## Evidence

- [S] `src-ui/src/app/components/app-frame/global-search/global-search.component.html:92-132` — row and button have different activation gates.
- [S] `src-ui/src/app/components/app-frame/global-search/global-search.component.ts:314-337` — workflow disable state is computed but not enforced by the action.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact permission-bypass report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/13865 — Related global-search timing fix; permission handling is distinct.

Contribution fit: New contribution — row activation remains inconsistent with its own disabled state.

## Proposed-Change

Route every primary activation through one permission-aware action boundary.

## Scope-and-Constraints

- Preserve: Authorized mouse, keyboard, and primary-button navigation.
- Exclude: Global-search result ranking or backend authorization.
- Cost: One local guard and interaction regression test.

## Verification

- `pnpm ng test --test-path-patterns=global-search.component.spec.ts --watch=false` → disabled workflow rows cannot invoke navigation.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

