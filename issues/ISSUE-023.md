# ISSUE-023 — Workflows: nested IDs bypass child permissions

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: API
Created: 2026-09-10
Updated: 2026-09-10
Source: `upstream/dev@a00755907e6fcb1467f50c5146d02e2b40efebb2`

## Root-Cause

Root-Cause [O]: Nested workflow writes pass client-supplied child IDs to global `update_or_create()` queries.
Root-Cause [O]: Those queries are not scoped to the workflow or action currently being updated.
Root-Cause [S]: The nested serializer path bypasses the direct child-model permission checks enforced by model viewsets.

## Reach-and-Impact

Reach [S]: A user who may create or change workflows can submit an existing trigger, action, email, or webhook ID.
Impact [S]: The request can mutate an unrelated existing child row instead of creating or updating only owned membership.
Impact [A]: The practical authorization gap depends on whether child permissions are intended below the global workflow role.
Impact [O]: Maintainer guidance describes workflows as global and high trust, so this is not a cross-tenant isolation claim.

## Evidence

- `serialisers.py:3451-3454` updates a trigger globally by client-supplied ID.
- `serialisers.py:3510-3533` updates actions globally by client-supplied IDs.
- `serialisers.py:3568-3574` updates nested email and webhook rows globally by client-supplied IDs.
- `views.py:4974-5068` exposes direct child endpoints through Django model permissions.
- `permissions.py:30-53` maps HTTP mutations to the corresponding model permissions.
- `models.py:1341-1991` defines the affected workflow child models as global records without an owner field.
- The affected serializer and permission code matched `upstream/dev` at the recorded source commit.

## Prior-Art

Coverage: Maintainer discussion documents the global workflow trust model but does not address foreign nested IDs.
Gaps: No exact public issue, pull request, advisory, or commit was found for this nested update behavior.

- https://github.com/paperless-ngx/paperless-ngx/discussions/5702 confirms that workflows run globally and require restricted editors.
- `issues/ISSUE-003.md` covers a frontend copy bug involving nested IDs, not authorization during API writes.
- https://github.com/paperless-ngx/paperless-ngx/pull/12390 scopes workflow filename changes, not nested child identity.
- https://github.com/paperless-ngx/paperless-ngx/security/policy treats expected high-trust workflow behavior separately from boundary bypasses.

Contribution fit: The serializer can enforce aggregate membership without changing the documented global workflow model.

## Proposed-Change

Treat nested IDs as references only when they already belong to the workflow aggregate being updated.
Create new child rows for create operations and reject IDs that refer to a different workflow or action.
Retain direct model-permission enforcement at the existing viewset boundary.

## Scope-and-Constraints

- Preserve: Authorized workflow editing and existing valid nested update payloads must continue to work.
- Exclude: Do not claim tenant isolation because the product intentionally models workflows as global.
- Cost: The serializer needs aggregate-scoped lookup rules and migration-safe handling of current nested payloads.

## Verification

- Use two users with deliberately different workflow and child-model permissions.
- Submit a nested update containing another workflow's trigger, action, email, and webhook IDs.
- Confirm that every foreign ID is rejected and that no unrelated row changes.
- Confirm that valid updates to children already belonging to the target workflow remain supported.

## Publication-Blockers

A two-user runtime reproduction and confirmation of the intended child-permission contract are still required.
The exact issue or advisory draft and publication target also require user approval.

## Next-Action

Summary: Reproduce permission bypass
Action: Exercise foreign nested IDs with users that separate workflow and child-model permissions.
Done-When: The API response and database state establish whether an unauthorized child mutation occurs.
