# Workflow v2 templates

Language / 语言: [简体中文](README.md) · **English**

These templates help people and dot turn requirements into changes that can be checked, reviewed, and handed off. **There is only one set of functional and operational templates; Chinese and English are two ways to read the documentation.** The package contains no automatic installer, CI scripts, or deployment functionality, and prescribes no toolchain for a particular language, product, or platform.

## Where to start

Hand the [Chinese Cookbook](template/COOKBOOK.md) or [English Cookbook](template/COOKBOOK.en.md) to dot on its own, filling in the target repository and task. Without a project, start with the local four-case red/green exercise.

- [Adoption and upgrades](docs/ADOPTION.en.md)
- [Single shared workflow specification (Chinese/English)](template/docs/workflow-v2.md)
- [Shared project rules template (Chinese/English)](template/AGENTS.md.template)
- [Changelog](CHANGELOG.en.md)

## Why it is designed this way

### Define success before writing the implementation

The specification states goals, non-goals, contracts, and observable acceptance criteria. It lets reviewers find ambiguities early, avoiding shifts in acceptance criteria after implementation. A spec records decisions; it does not mean the owner has already approved them.

### Replace “looks done” with actual evidence

Red before green verifies that the test can detect the target defect, then verifies that the minimal fix works. Red must be an assertion failure about expected behavior, not an environment installation or import error. After green, regression tests and full project checks are still required. For documentation-only changes, explain why behavioral tests are not applicable and validate links, content, examples, and privacy instead; do not fabricate red results.

### Separate implementation, self-review, and independent review

Implementers know their changes best but may overlook their own assumptions. Independent review checks the specification, code, tests, and evidence. CI establishes only the results of configured checks and cannot replace independent judgment. Independent internal analysis is not a formal APPROVE from another account on the platform.

### Tie gates to the actual candidate commit

Passing CI on an old commit does not establish that a new commit is safe. Record exact head/base SHAs and the tested commit. PR CI may test a synthetic merge commit, while push CI can test the original head. After head, base, or conflict resolution changes, revalidate affected checks and review validity to avoid merging on stale evidence.

### Authorization, validation, and release are different decisions

Repository access does not authorize permission changes; finished code does not authorize a push; green CI does not authorize merging or deployment. The templates record these states separately to avoid treating technical success as operational authorization. Stop when protections reject an action, rather than force-pushing or changing settings to get through.

### Traceable provenance without leaking private information

Commit provenance reflects actual contributions; do not describe human approval as human implementation. Authors/trailers are not cryptographic signatures. Shareable templates contain only generic rules, without private coordination, accounts, addresses, credentials, raw logs, or private history. Supplement scanning with human review.

## How a task runs from start to finish

1. **Locate and read:** confirm the target repository/environment, inspect the working tree, and preserve others' changes; read project rules, run instructions, and relevant specs.
2. **Specification and design review:** define goals, non-goals, inputs/outputs, invariants, risks, rollback, and acceptance mapping; obtain explicit approval first for major changes.
3. **Reproduce failure:** write acceptance tests first and record actual evidence of the target assertion failing; save a red-test commit when commits are allowed.
4. **Minimal implementation:** fix only the target problem; do not obtain green results by deleting tests or weakening assertions.
5. **Regression testing and checks:** run relevant tests and full project-required checks, distinguishing passed, failed, skipped, not run, and not configured.
6. **Independent review:** examine the current head or a clearly identified uncommitted snapshot; recheck affected areas after changes.
7. **PR after authorization:** create the task branch and Draft PR only when pushing and opening a PR are authorized; verify current CI, review, attribution, and protections, without automatically merging.
8. **Merge and delivery after authorization:** reread head/base and all gates before merging, then reread actual status afterward. Release/deployment requires separate authorization; archive acceptance results and remaining risks.

If independent review, remote CI, or authentication is missing, safe local preparation may continue, but explicitly mark missing items as blocked/not run. Do not claim that all gates passed.

## One shared template set, two documentation languages

The following files are the only adoption source. There is no separate English branch of operational rules. Each shared template contains Chinese and English; every change must keep their meanings aligned:

- [AGENTS.md.template](template/AGENTS.md.template): project rules.
- [workflow-v2.md](template/docs/workflow-v2.md): roles, end-to-end workflow, and gates.
- [Spec template](template/docs/specs/TEMPLATE.md): goals, contracts, tests, and risks.
- [PR template](template/PULL_REQUEST_TEMPLATE.md): scope, validation, approvals, and unfinished items.
- [Acceptance template](template/docs/ACCEPTANCE_TEMPLATE.md): requirement-to-evidence mapping.
- [Handoff template](template/docs/HANDOFF_TEMPLATE.md): status, decisions, next steps, and rollback.

The README, Cookbook, adoption guide, and changelog each provide Chinese and English reading versions, all referencing these six shared templates. Example code is identical; its behavior does not change with the documentation language. Before adoption, reconcile templates with existing project rules item by item, without overwriting those rules or duplicating them into two instruction sets. Both languages express the same rules and fields: keep filled-in values consistent and run the workflow once, not once per language.

## What makes acceptance complete

Delivery should include: scope of changes; environment and tool versions; exact commits or snapshots; requirement → test → result evidence; red/green and regression results; separate CI and independent-review status; attribution/privacy checks; unverified items, risks, rollback, and next steps. Report commits, pushes, PRs, merges, releases, and deployments separately. Do not describe “ready” as “live.”

If project-required checks are missing, failed, or pending, do not claim readiness to merge. A local exercise may be accepted as a local exercise; it does not establish validation of a real project, remote environment, or production.

## Adoption, sharing, and boundaries

Copy only the shared templates you need, filling in commands, roles, required checks, merge strategy, and release boundaries. Copying files does not enable CI, install services, or change permissions. New credentials, external review services, permissions, protections, sharing visibility, merging, and deployment must each follow user approval and project rules.

This repository is a generic, sanitized export without private coordination records. Review files and the history to be published before sharing; do not directly mirror a source repository containing private material. The templates do not replace a license agreement or automatically grant others repository access.

Current development version: 0.2.0 (unreleased). Pin a full commit when no verified release tag exists. Maintaining these templates does not automatically upgrade projects that adopt them.
