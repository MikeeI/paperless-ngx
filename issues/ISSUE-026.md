# ISSUE-026 — Container: native parser versions drift between builds

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Build
Created: 2026-09-10
Updated: 2026-09-10
Source: `upstream/dev@a00755907e6fcb1467f50c5146d02e2b40efebb2`

## Root-Cause

Root-Cause [O]: The container build installs native parsing packages from a moving Debian index without version constraints.
Root-Cause [S]: The same source commit can therefore resolve different parser binaries when built at different times.

## Reach-and-Impact

Reach [S]: Every container image built from the Dockerfile depends on the package versions available at build time.
Impact [S]: Reproducibility, defect attribution, and security assessment can drift independently of the Git commit.
Impact [A]: No specific vulnerable package or exploitable Paperless behavior has been established.
Impact [S]: Hard-pinning packages could also suppress security updates, so source-level pinning is not automatically safer.

## Evidence

- `Dockerfile:118-174` runs `apt-get update` and installs native packages without explicit versions.
- The list includes Ghostscript, ImageMagick, qpdf, Tesseract, Poppler, and libmagic components.
- Those binaries parse or transform untrusted document content outside Python dependency locking.
- No exact local ledger record covers native package provenance for container builds.
- No exact public issue or pull request was found for this Dockerfile behavior.

## Prior-Art

Coverage: Existing release artifacts may provide immutable image digests even when source rebuilds drift.
Gaps: Whether the release pipeline already publishes a complete SBOM or native package manifest remains unverified.

- https://github.com/paperless-ngx/paperless-ngx/security/policy excludes scanner-only third-party findings without product impact.

Contribution fit: Build provenance can improve without freezing native packages indefinitely in the Dockerfile.

## Proposed-Change

Record the produced image digest and generate an SBOM or native package manifest for each release artifact.
Use that artifact-level provenance as the security and reproduction reference for deployed binaries.
Avoid source-level package pinning unless the update and rebuild policy is defined at the same time.

## Scope-and-Constraints

- Preserve: Routine Debian security updates must remain available to maintained image builds.
- Exclude: Do not claim a Paperless vulnerability from package drift or scanner output alone.
- Cost: Release automation and artifact retention may need additional provenance generation and publication steps.

## Verification

- Build the same source revision through the supported release path at two controlled times.
- Capture image digests, `dpkg-query` output, and generated SBOMs for comparison.
- Confirm release consumers can map a deployed digest to exact native parser versions.
- Verify that the provenance mechanism does not prevent routine security rebuilds.

## Publication-Blockers

Existing release digest and SBOM coverage must be established before proposing an upstream change.
The contribution target and exact issue draft also require user approval.

## Next-Action

Summary: Capture native package provenance
Action: Inspect current release artifacts and compare native package manifests for identical source revisions.
Done-When: The evidence shows whether immutable digests and SBOMs already close the provenance gap.
