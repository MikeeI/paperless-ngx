# ISSUE-019 — Mail dialogs: failed delete disables retry

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: Mail delete handlers disable confirmation buttons before DELETE but do not re-enable them on error.

## Reach-and-Impact

Reach [S]: Affects failed mail-account and mail-rule deletion requests.
Impact [S]: The open dialog cannot retry or cancel without being recreated.

## Evidence

- [S] `src-ui/src/app/components/manage/mail/mail.component.ts:195-225,297-327` — error paths toast without restoring buttons.
- [S] `src-ui/src/app/components/common/confirm-dialog/confirm-dialog.component.html:20-35` — every footer control uses `buttonsEnabled`.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact failed-delete recovery report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/13672 — Related signal-wiring fix; it explicitly does not change failure behavior.

Contribution fit: New contribution — retry controls remain disabled after DELETE errors.

## Proposed-Change

Restore dialog buttons in each mail delete error callback.

## Scope-and-Constraints

- Preserve: Button locking while a request is active and modal dismissal on success.
- Exclude: Shared confirmation-dialog redesign.
- Cost: Two error-path assignments and focused tests.

## Verification

- `pnpm ng test --test-path-patterns=mail.component.spec.ts --watch=false` → failed account and rule deletions re-enable controls.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

