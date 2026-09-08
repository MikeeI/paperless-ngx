# ISSUE-002 — Saved views: multi-owner filters lose IDs

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-08
Updated: 2026-09-08
Source: `upstream/dev@b989b741401ad6a3dafbc565be2a28f08df6258e`

## Root-Cause

Root-Cause [S]: The filter editor serializes owner arrays as comma-separated IDs but parses the stored value with one `Number.parseInt` call.

## Reach-and-Impact

Reach [S]: Affects saved views containing multiple included or excluded owners.
Impact [S]: Reopening and emitting the view retains only the first owner ID.

## Evidence

- [S] `src-ui/src/app/components/document-list/filter-editor/filter-editor.component.ts:784-801,1136-1154` — encode and decode use incompatible cardinalities.
- [S] https://github.com/paperless-ngx/paperless-ngx/issues/13669 — Related dynamic-owner view behavior; not this parser mismatch.

## Prior-Art

Coverage: issues, pull requests, commits, discussions, releases; checked=2026-09-08.
Gaps: GitHub lexical search cannot prove absence under unrelated wording.

- https://github.com/paperless-ngx/paperless-ngx/issues/13669 — Related; shared `My documents` semantics have a different root cause.
- https://github.com/paperless-ngx/paperless-ngx/issues/12782 — Related; creator-relative ownership has a different root cause.

Contribution fit: New contribution — no exact multi-owner roundtrip fix was found.

## Proposed-Change

Decode every comma-separated owner ID with one shared filter-ID parser.

## Scope-and-Constraints

- Preserve: Single-owner and empty-owner filter behavior.
- Exclude: Saved-view schema changes.
- Cost: One parsing helper and focused regression test.

## Verification

- `pnpm ng test --test-path-patterns=filter-editor.component.spec.ts --watch=false` → `12,13` round-trips as both owners.

## Publication-Blockers

Runtime reproduction and an exact issue draft remain incomplete.

## Next-Action

Summary: Reproduce reported behavior
Action: Reproduce the source-backed behavior on canonical `dev` before drafting.
Done-When: Runtime evidence and remaining uncertainty are recorded.

