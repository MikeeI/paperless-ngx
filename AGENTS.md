# Repository Guidelines

## Project Overview

This repository is the personal contribution fork of Paperless-ngx, a community-supported document management system. The backend is a Django application in `src/`, the Angular frontend is in `src-ui/`, documentation is in `docs/`, and container and service integration is in `docker/` and `scripts/`. Paperless functional changes target the upstream `dev` branch.

## Fork & Upstream Contribution Intent

- Official upstream: [paperless-ngx/paperless-ngx](https://github.com/paperless-ngx/paperless-ngx).
- This checkout is the [MikeeI/paperless-ngx](https://github.com/MikeeI/paperless-ngx) fork, not an independently owned product.
- The local checkout follows the `project-<github-repository>-fork` naming convention as `project-paperless-ngx-fork`.
- The goal is to support upstream with evidence-backed issues, comments, and pull requests.
- `origin` is the personal fork and `upstream` is the canonical repository.
- `dev` is the current upstream contribution base; feature branches start from `upstream/dev`.
- `ISSUES.md` provides the compact finding overview and global ID allocator.
- `issues/ISSUE-NNN.md` owns the complete durable record for one root cause.
- `FORMAT.md` owns research, drafting, implementation authorization, approval, and publication rules.
- Apply `skill://skill-fork-contribution-tracking` for ledger, lifecycle, personal-branch, and upstream handoff work.
- Apply `skill-maintainer-communication` before external issues, pull requests, reviews, comments, or discussions.
- Keep fork-only context and ledgers out of upstream contribution diffs.
- Follow the upstream contribution guidance in `CONTRIBUTING.md` and `docs/development.md`.

## Finding and Contribution Ledger

- At the start of every agent session, agents MUST read root `ISSUES.md` before repository work.
- `ISSUES.md` owns the global `Next finding ID` allocator and compact cross-finding overview.
- Each `issues/ISSUE-NNN.md` owns one finding's state, evidence, drafts, and next action.
- `FORMAT.md` is authoritative for research, drafting, implementation boundaries, and publication format.
- Before adding a finding, search the index and every relevant issue record for the same symptom or root cause.
- New findings MUST use `Next finding ID`.
- Create the issue file, add its index row, and increment the allocator together.
- Finding IDs use `ISSUE-NNN`, start at `ISSUE-001`, and remain permanent.
- Update the issue file and `ISSUES.md` together after state, authorization, target, priority, next action, or reference changes.
- The user selects `Authorized-Work` and `Publication-Target` for each finding.
- Never publish without approval of the exact current draft and target.
- Run the read-only validator bundled with `skill-fork-contribution-tracking` after every ledger mutation.
- Keep `FORMAT.md`, `ISSUES.md`, `issues/`, and fork-only `AGENTS.md` changes out of upstream contribution diffs.

### External publication approval

Only an external issue, comment, review, discussion, or pull request write is approval-gated. Fork commits, pushes, tracking updates, and source implementation follow the active repository contract.
