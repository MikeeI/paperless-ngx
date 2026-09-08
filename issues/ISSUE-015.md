# ISSUE-015 — Workflows: failed toggle stays optimistic

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

Root-Cause [S]: Two-way binding mutates `workflow.enabled` before PATCH success, and the error path does not restore it.

## Reach-and-Impact

Reach [S]: Affects every failed workflow enable or disable request.
Impact [S]: The visible workflow state disagrees with the persisted server state.

## Evidence

- [S] `src-ui/src/app/components/manage/workflows/workflows.component.html:48-56` — checkbox state changes before persistence completes.
- [S] `src-ui/src/app/components/manage/workflows/workflows.component.ts:159-176` — PATCH errors only show a toast.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact failed-toggle recovery report was found.

- https://github.com/paperless-ngx/paperless-ngx/commit/4c4d3a45c2e058133f35ed4059b50a35dd9b06ff — Related toggle-jitter fix; it does not restore failed state.

Contribution fit: New contribution — workflow toggle errors remain optimistic.

## Proposed-Change

Use controlled binding, PATCH a candidate copy, and commit `enabled` only after success.

## Scope-and-Constraints

- Preserve: Successful toggle behavior and existing toast text.
- Exclude: Workflow edit-dialog behavior.
- Cost: Controlled binding and one failure regression test.

## Verification

- `pnpm ng test --test-path-patterns=workflows.component.spec.ts --watch=false` → failed toggles preserve prior visible state.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

