# Changelog

## 0.2.0 — Unreleased
- Upgrades the workflow to v2.1. The rules file is still named `template/docs/workflow-v2.md`; the version is given by the file title and the "Change log" at its end. Main changes:
  - Default roles and models: design and review default to Claude (Opus 5.5, High), coding defaults to Codex cloud (GPT-6.1 Sol, Extra High), and technical questions go to a technical research role that checks official sources. Projects may override these in the configuration table.
  - Technical research default (owner's decision): if the project has not configured a technical research role, the agent responsible for coding checks the official documentation itself and attaches source links.
  - Pre-start check: before starting design, review, or coding, confirm that the specified model and tier can be selected; if not, stop and report.
  - Per-batch loop: implement → strengthened tests → review → rework or proceed by S1/S2/S3 severity.
  - Stop points: in addition to major changes, lists the items that must wait for user approval; new dependencies need an architecture assessment first.
  - Access and credentials: inventory and request all permissions in one batch before starting; keys and credentials never land on the agent's work machine.
  - Task briefs: state the base commit, branch, Draft PR target branch, commit order, hard constraints, acceptance criteria, and attribution trailer.
  - Attribution trailer default (owner's decision): if the project has no attribution script or policy file, add one source trailer depending on where the coding agent runs. In the cloud, only the tool identifier (such as `Code-Tool: codex <model> <tier>`), with no Host-Id; only on a local computer, `Host-Id: <host-id>`. Documents use only placeholders, with a note that for local runs the host id appears in the public commit history.
  - Scanning and pushing: run them as two separate commands, and stop and report on any scan hit.
  - Adds the "Branches, PRs, and document delivery" and "Retrospectives and continuous learning" sections, plus a change log at the end of the file.
  - Adds three configuration rows: coding tool and tier, technical research role, and attribution trailer source; the last two state the default when the project has not configured them.
- Updates the AGENTS template to v2.1: adds the role and tier check, the technical research default, stop points, separate scan and push, and the trailer source with the Code-Tool/Host-Id default.
- Updates the README (Chinese and English) to v2.1: states the current version with a change summary; aligns roles, the loop, gate order, and the diagram with v2.1; moves severity grading of findings from "recommended" to "provided"; adds the technical research and attribution trailer defaults.
- Updates the Cookbook (Chinese and English) to v2.1: states the matching version; adds one-batch authorization, a task-brief template, per-batch severity handling, stop points, separate scan and push, retrospectives and reporting, and the technical research and attribution trailer defaults.
- Updates the adoption guide (Chinese and English) to v2.1: adds the new configuration items and their two defaults, v2 → v2.1 upgrade checks, and the difference between the template repository version and the workflow version.
- Adds a configurable cloud-first default, with boundaries for user-selected environments, local-resource dependencies and access restrictions.
- Clarifies verification of existing access, conditional truthful AI attribution with limited tools, and stopping for mandatory attribution-rule conflicts.
- Adds template/COOKBOOK.md, a standalone, shareable Cookbook that can be handed directly to dot.
- Provides complete Simplified Chinese and English reading documents; the six shared templates remain one set of bilingual files with identical rules, safety boundaries, and code semantics.
- Explains workflow design principles, the complete process, acceptance criteria, and authorization boundaries in the README.
- Provides minimum inputs, reading order, handling of missing information, role-based gates, specs, and acceptance records.
- Provides a four-case boundary-value red/green exercise with no third-party dependencies.
- Explains CI validation of the current commit, the difference between source attribution and cryptographic signatures, privacy-safe exports, and synchronization across repositories.
- Preserves all existing generic templates; does not configure CI, permissions, protections, or deployments.

## 0.1.0 — Initial export
- Generic workflow, adoption guide, spec, PR, acceptance, and handoff templates.
