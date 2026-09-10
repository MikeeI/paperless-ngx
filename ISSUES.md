# Issue and Pull Request Tracking

Read this index at the start of every agent session before repository work.
`FORMAT.md` owns research, lifecycle, drafting, implementation, and publication rules.
Each linked `issues/ISSUE-NNN.md` is the complete authoritative record for one root cause.
This file owns `Next finding ID` and projects current issue-file state.
`Next-Action` is the 2–6 word `Next-Action/Summary` projection from the issue record.
When a row disagrees with its issue file, correct the row from the issue file in the same task.

Next finding ID: ISSUE-027

## Open-Findings

| ID | Finding | State | Authorized-Work | Publication-Target | Contribution-Priority | Next-Action | External-Reference |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [ISSUE-001](issues/ISSUE-001.md) | Document detail: version switch overwrites dirty content | Submitted | Pull-Request-Implementation | New-pull-request | High | Monitor upstream pull request | https://github.com/paperless-ngx/paperless-ngx/pull/14062 |
| [ISSUE-002](issues/ISSUE-002.md) | Saved views: multi-owner filters lose IDs | Investigating | Research-and-Reporting | New-issue | High | Reproduce reported behavior | Not published. |
| [ISSUE-003](issues/ISSUE-003.md) | Workflows: copy mutates original nested IDs | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-004](issues/ISSUE-004.md) | Document selection: stale pruning response wins | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-005](issues/ISSUE-005.md) | Document selection: deleted IDs remain selected | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-006](issues/ISSUE-006.md) | Settings: deep routes bypass edit permission gate | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-007](issues/ISSUE-007.md) | Settings: failed save publishes unpersisted state | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-008](issues/ISSUE-008.md) | Tasks: stale requests overwrite current page | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-009](issues/ISSUE-009.md) | Bulk editor: failed download remains pending | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-010](issues/ISSUE-010.md) | Mail rules: failed toggle stays optimistic | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-011](issues/ISSUE-011.md) | Workflows: reload failure leaves loading active | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-012](issues/ISSUE-012.md) | Global search: workflow row bypasses permission gate | Investigating | Research-and-Reporting | New-issue | Low | Reproduce reported behavior | Not published. |
| [ISSUE-013](issues/ISSUE-013.md) | Filter editor: loading can remain active forever | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-014](issues/ISSUE-014.md) | Bulk editor: action permission gates are inconsistent | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-015](issues/ISSUE-015.md) | Workflows: failed toggle stays optimistic | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-016](issues/ISSUE-016.md) | Workflow editor: removal controls target wrong action | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-017](issues/ISSUE-017.md) | Document list: delete subscription outlives component | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-018](issues/ISSUE-018.md) | Mail permissions: failed PATCH mutates displayed object | Investigating | Research-and-Reporting | New-issue | Medium | Reproduce reported behavior | Not published. |
| [ISSUE-019](issues/ISSUE-019.md) | Mail dialogs: failed delete disables retry | Investigating | Research-and-Reporting | New-issue | Low | Reproduce reported behavior | Not published. |
| [ISSUE-020](issues/ISSUE-020.md) | Workflow dialog: failed delete disables retry | Investigating | Research-and-Reporting | New-issue | Low | Reproduce reported behavior | Not published. |
| [ISSUE-021](issues/ISSUE-021.md) | Document notes: concurrent deletes overwrite newer state | Investigating | Research-and-Reporting | New-issue | Low | Reproduce reported behavior | Not published. |
| [ISSUE-022](issues/ISSUE-022.md) | Importer: archive filename escapes media root | Investigating | Research-and-Reporting | Not-Selected | High | Reproduce importer overwrite | Not published. |
| [ISSUE-023](issues/ISSUE-023.md) | Workflows: nested IDs bypass child permissions | Investigating | Research-and-Reporting | Not-Selected | Medium | Reproduce permission bypass | Not published. |
| [ISSUE-024](issues/ISSUE-024.md) | Webhooks: internal requests are allowed by default | Investigating | Research-and-Reporting | Not-Selected | Low | Validate webhook threat model | Not published. |
| [ISSUE-025](issues/ISSUE-025.md) | OCR settings: user args override managed paths | Investigating | Research-and-Reporting | Not-Selected | Low | Probe OCR path overrides | Not published. |
| [ISSUE-026](issues/ISSUE-026.md) | Container: native parser versions drift between builds | Investigating | Research-and-Reporting | Not-Selected | Low | Capture native package provenance | Not published. |

## Archived-Findings

| ID | Finding | Authorized-Work | Publication-Target | Contribution-Priority | Archive-Reason | External-Reference |
| --- | --- | --- | --- | --- | --- | --- |
