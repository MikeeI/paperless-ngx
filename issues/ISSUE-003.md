# ISSUE-003 — Workflows: copy mutates original nested IDs

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

Root-Cause [S]: `copyWorkflow` copies arrays but mutates their shared action, trigger, email, and webhook objects.

## Reach-and-Impact

Reach [S]: Affects every workflow copy operation, including copies that are cancelled.
Impact [S]: The original list object loses nested IDs and a later PATCH can replace nested database rows.

## Evidence

- [S] `src-ui/src/app/components/manage/workflows/workflows.component.ts:102-124` — nested IDs are cleared on shared objects.
- [S] `src/documents/serialisers.py:3451-3454,3510-3533` — null nested IDs create replacement objects.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact complete copy-isolation report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/8729 — Related partial fix; it clears email and webhook IDs but retains shared mutation.
- https://github.com/paperless-ngx/paperless-ngx/issues/8994 — Distinct deletion-selection behavior.

Contribution fit: New contribution — complete source-object isolation remains absent.

## Proposed-Change

Deep-clone the workflow DTO before clearing IDs on the copy.

## Scope-and-Constraints

- Preserve: Copy names, action order, trigger order, and nested payloads.
- Exclude: Workflow serializer redesign.
- Cost: One clone boundary and source-identity regression test.

## Verification

- `pnpm ng test --test-path-patterns=workflows.component.spec.ts --watch=false` → cancelling a copy leaves every original ID unchanged.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

