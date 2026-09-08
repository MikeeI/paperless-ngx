# ISSUE-021 — Document notes: concurrent deletes overwrite newer state

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

Root-Cause [S]: Concurrent note deletions each replace local state with a full server snapshot, so response order becomes state order.

## Reach-and-Impact

Reach [S]: Affects users who trigger a second note deletion before the first response returns.
Impact [S]: A late older snapshot can visually resurrect a note already deleted by the newer request.

## Evidence

- [S] `src-ui/src/app/components/document-notes/document-notes.component.ts:94-105` — every delete response replaces the complete notes signal.
- [S] `src-ui/src/app/services/rest/document-notes.service.ts:34-39` — DELETE returns a full notes-array snapshot.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: No exact concurrent mutation-ordering report was found.

- https://github.com/paperless-ngx/paperless-ngx/pull/12582 — Related note-delete validation fix; concurrency is distinct.

Contribution fit: New contribution — note mutations lack single-owner ordering.

## Proposed-Change

Allow only one note mutation at a time and disable delete controls while the mutation is active.

## Scope-and-Constraints

- Preserve: Server-returned authoritative note snapshots and successful add/delete behavior.
- Exclude: Optimistic note reconciliation.
- Cost: One mutation guard and interaction regression test.

## Verification

- `pnpm ng test --test-path-patterns=document-notes.component.spec.ts --watch=false` → a second delete cannot start during an active mutation.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

