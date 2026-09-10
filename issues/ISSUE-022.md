# ISSUE-022 — Importer: archive filename escapes media root

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Other
Created: 2026-09-10
Updated: 2026-09-10
Source: `upstream/dev@a00755907e6fcb1467f50c5146d02e2b40efebb2`

## Root-Cause

Root-Cause [O]: The document importer persists manifest-controlled filenames before validating their resolved paths.
Root-Cause [O]: Archive copies do not enforce destination containment or reject an existing destination.
Root-Cause [O]: Source references are joined to the import directory without verifying source containment.
Root-Cause [S]: `bulk_create()` bypasses model validation, so no later boundary restores these path invariants.

## Reach-and-Impact

Reach [S]: An actor who can supply and import a crafted Paperless export archive can reach the affected paths.
Impact [O]: A crafted archive filename can write outside the archive root and overwrite an existing writable file.
Impact [O]: A crafted source reference can make the importer read and copy a readable file outside the import directory.
Impact [A]: Code execution may follow if a writable imported module is later loaded by the running Paperless process.
Impact [A]: Standard-container permissions and a reliable module-loading path have not been verified end to end.

## Evidence

- `document_importer.py:90-140` deserializes records without model or resolved-path validation.
- `document_importer.py:543-585` resolves manifest source references relative to `self.source` without containment checks.
- `document_importer.py:618-666` copies archived files without destination containment or an existing-file guard.
- `models.py:276-284,452-455` derives the archive path directly from the persisted `archive_filename`.
- `utils.py:68-116` resolves that destination and delegates to `shutil.copy()`.
- `document_importer.py:536-541` triggers a reindex command after imported records are created.
- An isolated core reproduction observed `absolute_path_escapes_archive_root=true`.
- The same reproduction observed `existing_target_overwritten=true`.
- The same reproduction observed `module_import_executed_payload=true` for a deliberately loaded overwritten module.
- The affected importer and path code matched `upstream/dev` at the recorded source commit.

## Prior-Art

Coverage: Public advisories cover related arbitrary-file-write and traversal classes, but not this importer boundary.
Gaps: No exact public issue, pull request, advisory, or commit was found for manifest-controlled importer paths.

- https://github.com/paperless-ngx/paperless-ngx/security/advisories/GHSA-28cf-xvcf-hw6m covers a distinct Storage Paths file-write root cause.
- https://github.com/paperless-ngx/paperless-ngx/security/advisories/GHSA-2jhj-xqrq-rmrq covers a distinct filename-normalization traversal.
- https://github.com/paperless-ngx/paperless-ngx/issues/4485 covers Unicode normalization during import, not path containment.
- https://github.com/paperless-ngx/paperless-ngx/security/policy requires a private report with a real reproduction and impact.

Contribution fit: The importer owns the trust-boundary failure and can reject unsafe paths without redesigning storage.

## Proposed-Change

Validate every manifest-controlled source reference and persisted document filename before any database or file mutation.
Resolve each path against its owning root, reject absolute or escaping paths, and reject unsafe existing destinations.
Keep validation in the importer because its bulk insertion path bypasses model validation.

## Scope-and-Constraints

- Preserve: Valid exports, archive imports, and the existing storage-layout contract must remain compatible.
- Exclude: Do not treat external-file copying as data exfiltration unless an attacker can receive the copied content.
- Cost: The importer needs explicit path-policy checks and compatibility coverage for valid historical exports.

## Verification

- Import a crafted export whose `archive_filename` resolves outside the archive root and assert rejection before mutation.
- Import a crafted export whose source reference escapes the import root and assert rejection before any external read.
- Run the same scenarios in the supported container to establish effective permissions and remaining impact.
- Import a representative valid historical export and confirm unchanged document and archive restoration.

## Publication-Blockers

A current-upstream end-to-end importer reproduction and standard-container permission evidence are still required.
The exact private advisory draft and publication target also require user approval.

## Next-Action

Summary: Reproduce importer overwrite
Action: Build a minimal current-upstream export archive that exercises both source and destination traversal paths.
Done-When: The supported container records the rejected or successful write, effective permissions, and exact affected paths.
