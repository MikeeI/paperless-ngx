# ISSUE-007 — Settings: failed save publishes unpersisted state

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

Root-Cause [S]: `saveSettings` mutates the shared singleton before persistence and resets `savePending` only on success.

## Reach-and-Impact

Reach [S]: Affects every settings POST that fails after local form values are applied.
Impact [S]: Consumers observe values the server rejected, and `savePending` remains stuck.

## Evidence

- [S] `src-ui/src/app/components/admin/settings/settings.component.ts:451-618` — local mutations precede POST and error recovery.
- [S] `src-ui/src/app/services/settings.service.ts:609-636` — `set` publishes immediately while `storeSettings` persists later.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact rollback or `savePending` report was found.

- https://github.com/paperless-ngx/paperless-ngx/issues/4820 — Distinct theme-color failure.
- https://github.com/paperless-ngx/paperless-ngx/discussions/4586 — Distinct deployment-related `403` behavior.

Contribution fit: New contribution — settings persistence has no atomic UI commit boundary.

## Proposed-Change

Persist a candidate settings payload, commit it to shared state only on success, and finalize `savePending` on every outcome.

## Scope-and-Constraints

- Preserve: Current settings schema, signals, success toast, and reload notice.
- Exclude: Global state-management replacement.
- Cost: A transactional settings-service method and focused failure test.

## Verification

- `pnpm ng test --test-path-patterns=settings.component.spec.ts --watch=false` → failed saves preserve prior shared state and clear `savePending`.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

