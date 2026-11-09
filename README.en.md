# Workflow v2 templates

Language / 语言: [简体中文](README.md) · **English**

Development collaboration templates for people and AI: start with requirements and design review, then implement, validate and independently review changes to produce results that can be accepted, traced and handed off. **The core is clear responsibilities, evidence for progress, and stopping when an owner’s decision is needed.**

To start directly, hand the [Cookbook](template/COOKBOOK.en.md) to dot on its own, then provide a target repository and task. This README explains the design: why the method exists, who is responsible, when checks happen and where to adopt it.

## Why · Why use this workflow?

When people or multiple AI tools take turns developing a project, “the code is written” is not enough. They need a shared understanding of the requirements, evidence that tests cover the goal, a record of independent review, clarity about authorized actions, and next steps for whoever takes over.

Workflow v2 addresses these needs through one collaboration process:

- **Align before implementing:** define goals, non-goals, inputs, outputs and acceptance criteria; review requirements and design before discovering too late that the implementation went in the wrong direction.
- **Prove results with evidence:** first show that a test detects the target problem, then make the minimal implementation and run regression checks. Link each acceptance item to a test or check result.
- **Check implementation independently:** implementer self-review, independent review and CI serve different purposes; none should be presented as another. CI runs the project’s configured deterministic checks; AI review is recorded separately.
- **Leave work that others can continue:** record the current commit, completed and unfinished work, risks and next steps so a different person or session can pick it up.

These templates organize collaboration. They do not prescribe a model, programming language, product or CI platform, or automatically install tools, enable services or deploy a project.

## Who · Who is responsible for what?

Roles are defined by responsibility. Each adopting project assigns people or AI tools to them:

- **Requirements and architecture reviewer:** checks the problem, design, contracts, risks and acceptance criteria.
- **Implementer:** writes tests, implements and debugs changes, runs project checks, and provides actual evidence.
- **Independent reviewer:** compares the current changes and evidence with the specification; does not substitute implementer self-review for independent judgment.
- **Owner:** confirms scope, decides on major changes, merging and releases, and identifies who follows up and archives the work.

A person requests the task or explicitly authorizes its trigger. AI collaborates within that scope; completing one stage does not expand permission to merge or deploy.

No role requires a particular AI brand. An independent internal review can provide analysis, but it is not formal approval by another account on the code-hosting platform. A completed specification for a major change is not the owner’s approval.

## What · What does it provide, and what gets delivered?

The repository provides **one shared set of bilingual operational templates**, not separate Chinese and English workflows:

- [Project rules](template/AGENTS.md.template): responsibilities, commands and boundaries after adoption.
- [Workflow specification](template/docs/workflow-v2.md): the collaboration process, reviews, validation and approval requirements.
- [Specification template](template/docs/specs/TEMPLATE.md): goals, contracts, acceptance, risks and rollback.
- [PR template](template/PULL_REQUEST_TEMPLATE.md): the changes, validation results and outstanding work.
- [Acceptance template](template/docs/ACCEPTANCE_TEMPLATE.md): requirements mapped to evidence.
- [Handoff template](template/docs/HANDOFF_TEMPLATE.md): current status, decisions, next steps and rollback.

The README, [Cookbook](template/COOKBOOK.en.md), [adoption guide](docs/ADOPTION.en.md) and [changelog](CHANGELOG.en.md) each have Chinese and English reading versions. They share the six templates above. Keep values and meanings consistent; run the workflow once.

A task’s delivery should tell the next person what changed, which commit or snapshot it refers to, how it was validated, who reviewed it, what remains, and the risks and next steps. Report commits, pushes, PRs, merges, releases and deployments separately; do not describe “prepared” as “live.”

## When · When are decisions and checks needed?

- **Before starting:** confirm the target, scope and authorization; read project rules, inspect the existing workspace and preserve others’ changes.
- **Before implementation:** write the specification and review the design; obtain explicit owner approval for major changes.
- **During a fix:** record the target behavior’s assertion failure first, make the minimal fix, then run regression tests and all project-required checks. For documentation-only changes, validate content, links, examples and privacy rather than fabricating red tests.
- **At review:** independent review must correspond to the current head or an identified file snapshot; recheck affected areas after changes.
- **Before pushing or opening a PR:** verify authorization for both actions; use a task branch and Draft PR.
- **Before merging:** separately confirm merge authorization and project strategy; reread head/base, current CI, valid review, attribution and protections. Checks on an older commit do not establish evidence for the current one.
- **Before release or deployment:** obtain the relevant authorization; local validation does not establish remote or production validation. Reread the actual outcome afterward, then archive the delivery record.

If review, authentication or required checks are missing, safe, authorized preparation may continue. Mark missing items as blocked, not run or not configured; do not claim that all checks passed or that the work is ready to merge. Repository access alone does not authorize permission changes, merging or deployment.

## Where · Where is it adopted and run?

Use the templates inside an actual project: copy only the needed files, reconcile them with existing rules, and fill in the project’s test commands, review mechanism, required checks and merge strategy. See the [adoption guide](docs/ADOPTION.en.md) for paths and upgrades. Do not overwrite existing rules or install duplicate sets for the two languages.

Execution has a **configurable cloud-first default**. Follow the user’s chosen environment; when a task depends on local files, apps or signed-in sessions, use a suitable environment with authorized access to those resources.

Verify existing repository access and tool capabilities first. Prefer an already-connected API/connector that supports the operation. A CLI without a login does not imply missing connector access and is not grounds for duplicate authorization requests. Distinguish unsupported capabilities from denied access; never switch environments, accounts or paths to evade restrictions.

Preserve truthful contribution origin in the project: humans use their chosen identity without mandatory noreply, and human approval does not turn AI work into human work. When a tool cannot set an AI author, retain the actual author/committer only where project rules permit that fallback, and use explicit AI authored text, an Agent trailer and PR disclosure to explain origin and limitations. Stop and ask if mandatory identity, DCO or signing requirements cannot be met. Provenance declarations are not cryptographic signatures; see the [Cookbook attribution section](template/COOKBOOK.en.md#7-attribution-signatures-and-provenance).

Share reusable methods, not private coordination, credentials, raw logs or private history. Copying templates does not change repository visibility or grant access; review files and the history to be published before sharing. These templates do not replace a license agreement.

## How · Start with one task

Give dot the [Chinese Cookbook](template/COOKBOOK.md) or [English Cookbook](template/COOKBOOK.en.md), a target repository and a concrete task. It contains a starting prompt, guidance for missing information and step-by-step instructions. Without a project, start with its four-case local red/green exercise; that does not establish validation of a real project or production.

The shortest path is: **confirm the task → specification and design review → failing reproduction → minimal implementation → regression checks → independent review → delivery within the authorized scope**. Merging and deployment are not automatic next steps.

Current development version: 0.2.0 (unreleased). Pin a full commit when no verified release tag exists; updating this template does not automatically upgrade projects that adopt it.
