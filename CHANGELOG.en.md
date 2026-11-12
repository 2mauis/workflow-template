# Changelog

## 0.2.0 — Unreleased
- Upgrades the workflow to v2.1 (file name stays template/docs/workflow-v2.md): default roles and models, the per-batch loop with S1/S2/S3 handling, stop points that require user approval, task-brief requirements, separate scan and push steps, branch and document delivery, retrospectives and continuous learning, plus an in-document change log; the AGENTS template now references v2.1.
- Adds a configurable cloud-first default, with boundaries for user-selected environments, local-resource dependencies and access restrictions.
- Clarifies verification of existing access, conditional truthful AI attribution with limited tools, and stopping for mandatory attribution-rule conflicts.
- Provides complete Simplified Chinese and English reading documents; the six shared templates remain one set of bilingual files with identical rules, safety boundaries, and code semantics.
- README explains workflow design principles, the complete process, acceptance criteria, and authorization boundaries.
- Added template/COOKBOOK.md, a standalone, shareable Cookbook that can be handed directly to dot.
- Provides minimum inputs, reading order, handling of missing information, role-based gates, specs, and acceptance records.
- Provides a four-case boundary-value red/green exercise with no third-party dependencies.
- Explains CI validation of the current commit, the difference between source attribution and cryptographic signatures, privacy-safe exports, and synchronization across repositories.
- Preserves all existing generic templates; does not configure CI, permissions, protections, or deployments.

## 0.1.0 — Initial export
- Generic workflow, adoption guide, spec, PR, acceptance, and handoff templates.
