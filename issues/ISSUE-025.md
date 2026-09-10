# ISSUE-025 — OCR settings: user args override managed paths

State: Investigating
Authorized-Work: Research-and-Reporting
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: Medium
Finding-Category: API
Created: 2026-09-10
Updated: 2026-09-10
Source: `upstream/dev@a00755907e6fcb1467f50c5146d02e2b40efebb2`

## Root-Cause

Root-Cause [O]: Application configuration exposes arbitrary OCR `user_args` through the API.
Root-Cause [O]: Tesseract merges those arguments after Paperless constructs its managed OCR arguments.
Root-Cause [S]: Last-write-wins merging permits replacement of input, output, and sidecar path keys.

## Reach-and-Impact

Reach [S]: A user with `change_applicationconfiguration` can store crafted arguments for a later OCR task.
Impact [A]: OCRmyPDF may read from or write to worker paths chosen through overwritten lifecycle arguments.
Impact [A]: The real library behavior, worker permissions, and resulting product-level impact are not yet reproduced.
Impact [O]: The required application-configuration permission is administrative and therefore already high trust.

## Evidence

- `serialisers.py:213-255` exposes application-configuration `user_args` as writable JSON.
- `views.py:412-419` protects the endpoint with authentication and Django model permissions.
- `config.py:101-110` loads persisted OCR arguments from application configuration.
- `tesseract.py:265-277` constructs Paperless-owned input, output, and sidecar paths.
- `tesseract.py:323-325,359-366` adds other managed OCR lifecycle arguments.
- `tesseract.py:592-603` merges user arguments last and passes the result to `ocrmypdf.ocr()`.
- The affected configuration, serializer, and parser code matched `upstream/dev` at the recorded source commit.

## Prior-Art

Coverage: Public reports confirm that `user_args` intentionally exposes advanced OCRmyPDF options.
Gaps: No exact public security report was found for overriding Paperless-owned path arguments.

- https://github.com/paperless-ngx/paperless-ngx/issues/4971 covers advanced OCR arguments and fallback interaction.
- https://github.com/paperless-ngx/paperless-ngx/discussions/11939 discusses trusted extra arguments for external OCR.
- https://github.com/paperless-ngx/paperless-ngx/security/policy treats expected application-configuration access as high trust.

Contribution fit: Reserved-key filtering can retain advanced OCR tuning while preserving Paperless path ownership.

## Proposed-Change

Define the Paperless-owned OCR lifecycle keys in the parser and reject their presence in configured `user_args`.
Continue passing supported OCR tuning options through unchanged.
Reject invalid configuration at the API boundary rather than waiting for an asynchronous OCR task.

## Scope-and-Constraints

- Preserve: Administrators must retain access to legitimate advanced OCRmyPDF tuning options.
- Exclude: Do not assert arbitrary file access until real OCRmyPDF behavior and permissions reproduce it.
- Cost: Existing configurations using reserved keys need an explicit validation error and migration guidance.

## Verification

- Submit each Paperless-owned input, output, and sidecar key through the real application-configuration API.
- Run a representative OCR task and observe the arguments and filesystem effects at the library boundary.
- Confirm reserved keys are rejected while a supported advanced OCR option still reaches OCRmyPDF.
- Record worker permissions and cleanup behavior for every created path.

## Publication-Blockers

An isolated OCRmyPDF reproduction and confirmation of the intended application-configuration trust contract are required.
Concrete product-level impact and the exact publication target also remain unselected.

## Next-Action

Summary: Probe OCR path overrides
Action: Exercise reserved path keys through the real API and OCR task boundary.
Done-When: Observed library calls and filesystem effects establish whether managed paths can be displaced.
