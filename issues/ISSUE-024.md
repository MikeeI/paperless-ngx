# ISSUE-024 — Webhooks: internal requests are allowed by default

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Other
Created: 2026-09-10
Updated: 2026-09-10
Source: `upstream/dev@a00755907e6fcb1467f50c5146d02e2b40efebb2`

## Root-Cause

Root-Cause [O]: `WEBHOOKS_ALLOW_INTERNAL_REQUESTS` defaults to `true`.
Root-Cause [O]: The pinned-host transport rejects non-public destinations only when that setting is disabled.
Root-Cause [O]: No destination-port restriction applies when internal requests are enabled.

## Reach-and-Impact

Reach [S]: An authenticated workflow editor can configure a webhook and trigger its matching workflow.
Impact [S]: The worker can issue a blind HTTP POST to loopback, private, link-local, or other internal destinations.
Impact [S]: The request may optionally attach the matched document to the configured internal destination.
Impact [A]: Unauthorized product-level impact is unproven because workflow editors are intentionally high-trust users.

## Evidence

- `settings/__init__.py:1194-1207` defines the internal-request setting with a default of `true`.
- `webhooks.py:19-65` constructs the pinned-host transport from that setting.
- `network.py:61-160` permits resolved internal addresses and unrestricted ports when the setting is enabled.
- `actions.py:189-267` sends the configured POST request and can attach document content.
- `configuration.md:1546-1568` documents internal requests as enabled by default.
- The affected settings, transport, and webhook code matched `upstream/dev` at the recorded source commit.

## Prior-Art

Coverage: A prior advisory fixed DNS rebinding when internal requests were disabled.
Gaps: No exact public report was found challenging the current default policy itself.

- https://github.com/paperless-ngx/paperless-ngx/security/advisories/GHSA-6653-vcx4-69mc covers a distinct DNS-rebinding bypass fixed in 2.20.2.
- https://github.com/paperless-ngx/paperless-ngx/discussions/9442 demonstrates intended localhost webhook use.
- https://github.com/paperless-ngx/paperless-ngx/discussions/9020 demonstrates intended internal-service webhook use.
- https://github.com/paperless-ngx/paperless-ngx/security/policy treats expected privileged webhook behavior as non-vulnerable without product-level impact.

Contribution fit: A safer default can preserve explicit internal integration support through operator opt-in.

## Proposed-Change

Default internal webhook requests to disabled while retaining the existing explicit configuration switch.
Document the trust consequence and migration requirement for operators who intentionally target internal services.

## Scope-and-Constraints

- Preserve: Operators must retain an explicit way to use loopback and private webhook destinations.
- Exclude: Do not classify intended high-trust workflow behavior as a vulnerability without unauthorized impact.
- Cost: Changing the default can break existing internal integrations unless release notes provide a clear migration path.

## Verification

- Confirm the default deployment rejects loopback, private, link-local, and metadata-service destinations.
- Confirm explicit operator opt-in restores intentional internal webhook delivery.
- Confirm redirect and DNS-rebinding defenses remain effective in both modes.
- Establish whether a lower-privileged actor can obtain workflow-editing access in a supported configuration.

## Publication-Blockers

A concrete unintended product-level impact beyond the documented privileged feature is still required for security treatment.
The exact issue draft and publication target also require user approval.

## Next-Action

Summary: Validate webhook threat model
Action: Map workflow-edit permissions in supported deployments and test representative internal destinations.
Done-When: Evidence distinguishes an unsafe default from expected high-trust operator functionality.
