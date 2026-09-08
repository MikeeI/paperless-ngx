# ISSUE-018 — Mail permissions: failed PATCH mutates displayed object

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

Root-Cause [S]: `editPermissions` assigns owner and permission payload fields directly to the displayed account or rule before PATCH succeeds.

## Reach-and-Impact

Reach [S]: Affects failed permission edits for mail accounts and mail rules.
Impact [S]: The UI publishes permission state the server rejected until a reload.

## Evidence

- [S] `src-ui/src/app/components/manage/mail/mail.component.ts:331-359` — the shared object is mutated before persistence.
- [S] https://github.com/paperless-ngx/paperless-ngx/pull/12571 — Related permission payload work; it does not isolate failed PATCH state.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact mutation-on-failure report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/12571 — Related; non-owner payload suppression has a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/pull/14023 — Related retrieval error handling; mutation persistence is distinct.

Contribution fit: New contribution — permission edits still mutate shared state before success.

## Proposed-Change

PATCH an isolated candidate object and merge the server response only after success.

## Scope-and-Constraints

- Preserve: Permission dialog payload and successful object refresh.
- Exclude: Permission schema changes.
- Cost: One immutable request boundary and failure regression test.

## Verification

- `pnpm ng test --test-path-patterns=mail.component.spec.ts --watch=false` → failed permission PATCH leaves the displayed object unchanged.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

