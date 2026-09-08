# ISSUE-001 — Document detail: version switch overwrites dirty content

State: PR-Ready
Authorized-Work: Pull-Request-Implementation
Publication-Target: New-pull-request
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: `selectVersion` replaces the editable content control without guarding its dirty state.

## Reach-and-Impact

Reach [S]: Affects users who edit document content and select another file version before saving.
Impact [S]: The asynchronous version response replaces the unsaved content value without confirmation.

## Evidence

- [S] `src-ui/src/app/components/document-detail/document-detail.component.ts:493-525,948-993` — dirty tracking and version loading share the content form control without a guard.
- [S] https://github.com/paperless-ngx/paperless-ngx/pull/12233 — Related version-editing work; it does not protect dirty content.
- [O] `corepack pnpm ng test --test-path-patterns=document-detail.component.spec.ts --watch=false` — 116 tests passed, including dirty version-switch confirmation and accepted-content baseline coverage.
- [O] Disposable backend and Angular E2E UI in Chromium — selecting the current version with edited content displayed the confirmation; `Keep editing` preserved the content and selected version.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: GitHub lexical search cannot prove absence under unrelated wording.

- https://github.com/paperless-ngx/paperless-ngx/pull/12233 — Related; selected-version edit targeting has a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/issues/13892 — Distinct; backend version metadata handling has a different root cause.

Contribution fit: New contribution — no exact dirty-content version-switch fix was found.

## Proposed-Change

Protect version switching with the existing dirty-state confirmation and reset the content baseline after an accepted switch.

## Scope-and-Constraints

- Preserve: Version preview, download, metadata, and explicit selected-version behavior.
- Exclude: General document dirty-state redesign.
- Cost: One additional confirmation path and focused component test.

## Verification

- `corepack pnpm ng test --test-path-patterns=document-detail.component.spec.ts --watch=false` → passed, 116 tests.
- `corepack pnpm lint` → passed.
- Chromium desktop with disposable E2E backend → confirmation rendered; cancel preserved unsaved content and selected version.

## Publication-Blockers

External publication requires approval of the exact pull request draft below.

## Next-Action

Summary: Review pull request draft
Action: Review the exact draft and target before external publication.
Done-When: The user approves the exact draft for submission to `paperless-ngx/paperless-ngx`.

## Pull-Request-Implementation

Branch: `fix/document-version-dirty-content`
Base: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`
Scope: Protect dirty document content during version selection.
Commit: `fe67c09ea77e101131c6f97ced453ffdbbb3a47c`
Push: `origin/fix/document-version-dirty-content@fe67c09ea77e101131c6f97ced453ffdbbb3a47c`
Checks:

- `corepack pnpm ng test --test-path-patterns=document-detail.component.spec.ts --watch=false` — passed, 116 tests.
- `corepack pnpm lint` — passed.
- Chromium desktop with disposable E2E backend — passed confirmation and cancel-preservation scenario.

## Publication-Draft

Target: New pull request against `paperless-ngx/paperless-ngx:dev` from `MikeeI/paperless-ngx:fix/document-version-dirty-content`.
Title: `Fix unsaved content loss when switching document versions`
Body:

```markdown
ASLOP-PR-VERIFY

## Proposed change

Switching between document file versions can replace unsaved edits in the content field without warning.
This change asks for confirmation before replacing dirty content and keeps the current selection when canceled.
After an accepted switch, it updates the dirty-check baseline for content while preserving unrelated dirty fields.

Testing:

- `pnpm ng test --test-path-patterns=document-detail.component.spec.ts --watch=false`
- `pnpm lint`
- Chromium desktop against the disposable E2E backend: the confirmation rendered and canceling preserved unsaved content and the selected version.

This pull request was prepared with assistance from an AI coding agent under human direction.

## Type of change

- [x] Bug fix: non-breaking change which fixes an issue.
- [ ] New feature / Enhancement: non-breaking change which adds functionality. _Please read the important note above._
- [ ] Breaking change: fix or feature that would cause existing functionality to not work as expected.
- [ ] Documentation only.
- [ ] Other. Please explain:

## Checklist:

- [x] I have read & agree with the [contributing guidelines](https://github.com/paperless-ngx/paperless-ngx/blob/main/CONTRIBUTING.md).
- [x] If applicable, I have included testing coverage for new code in this PR, for [backend](https://docs.paperless-ngx.com/development/#testing) and / or [front-end](https://docs.paperless-ngx.com/development/#testing-and-code-style) changes.
- [ ] If applicable, I have tested my code for breaking changes & regressions on both mobile & desktop devices, using the latest version of major browsers.
- [ ] If applicable, I have checked that all tests pass, see [documentation](https://docs.paperless-ngx.com/development/#back-end-development).
- [ ] I have run all Git `pre-commit` hooks, see [documentation](https://docs.paperless-ngx.com/development/#code-formatting-with-pre-commit-hooks).
- [x] I have made corresponding changes to the documentation as needed.
- [x] In the description of the PR above I have clearly disclosed any use of AI tools or agents in the creation of this PR. I understand that failing to do so is a violation of the [Code of Conduct](https://github.com/paperless-ngx/paperless-ngx/blob/main/CODE_OF_CONDUCT.md).
```
