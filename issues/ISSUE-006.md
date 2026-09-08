# ISSUE-006 — Settings: deep routes bypass edit permission gate

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: UI
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: `/settings/:section` requires only `View` while rendering the same writable form as the `Change`-guarded root route.

## Reach-and-Impact

Reach [S]: Affects custom roles with UI-settings view permission but no edit permission.
Impact [S]: A direct deep link exposes editable controls and a Save request that the permission policy does not intend.

## Evidence

- [S] `src-ui/src/app/app-routing.module.ts:211-234` — root and section routes use different actions for one component.
- [S] `src-ui/src/app/components/admin/settings/settings.component.html:407-408` — Save has no permission gate.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: Exact deep-route behavior was not covered by the located fix.

- https://github.com/paperless-ngx/paperless-ngx/issues/13418 — Related exact permission-affordance class; fixed for attribute controls.
- https://github.com/paperless-ngx/paperless-ngx/pull/13425 — Related fix; establishes that UI-settings edit controls require edit permission.
- https://github.com/paperless-ngx/paperless-ngx/pull/5919 — Related policy evidence; settings UI must not allow editing without model permission.

Contribution fit: New contribution — the section-route and Save gate remain inconsistent.

## Proposed-Change

Require the edit action on section routes and guard Save with the persistence permissions.

## Scope-and-Constraints

- Preserve: Existing settings sections and authorized navigation.
- Exclude: Backend permission-model redesign.
- Cost: Route metadata and Save affordance checks.

## Verification

- `pnpm ng test --test-path-patterns=permissions.guard.spec.ts --watch=false` → deep routes reject view-only users.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

