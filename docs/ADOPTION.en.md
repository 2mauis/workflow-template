# Adoption and upgrades

Read the [standalone Cookbook for dot](../template/COOKBOOK.en.md) first. You do not need to install every template just to start a task.

The Chinese and English adoption guides both refer to the same bilingual shared templates in `template/`. Each field has one meaning and one value. Keep both language expressions consistent; do not adopt two separate rule sets.

## Initial adoption
1. If a verified release tag exists, record both the tag and full commit. Otherwise, pin the full commit and mark it as unreleased. Do not depend on the movable main branch.
2. Copy only the needed files from `template/` into the project. Do not copy private-context or this repository's history.
3. Save `AGENTS.md.template` as the project's AGENTS.md. Put the PR template in a location recognized by the target platform (such as `.github/pull_request_template.md` or `.gitea/pull_request_template.md`); verify platform support before adopting it.
4. Fill in project commands, test paths, independent review mechanisms, required checks, release boundaries, and merge strategy. Mark nonexistent checks as “not configured”; never report them as passed.
5. Merge through the project's own review and approval process, and record the source template version. Copying files does not activate a deployment workflow.
6. Validate with a small task first: reproduce failure → implement → regression tests → independent review → exact SHA checks. Release/deployment requires separate project authorization.

## Configurable choices and important distinctions
- GitHub and Gitea differ in approvals, protections, checks, and review App capabilities. Do not assume they are equivalent.
- Do not install Apps, create tokens, or change permissions or required checks by default.
- Fast-forward merging that preserves original commits is an optional strategy, not the default for every project. Stop if protection requirements cannot be met; do not switch to a bypass.
- Documentation-only changes may mark behavioral tests as not applicable with an explanation. Do not fabricate red/green evidence.
- Even private staging should exclude private coordination information. Releases must use the project's own explicit allowlist and privacy checks, without private history.

## Upgrades and rollbacks
Use a separate PR to compare two pinned commits (include tags if available), transfer only applicable rules, and record conflicts and adoption reasons. Roll back to the previous verified version through a new revert commit, without force-pushing or deleting history. Track template versions separately from the versions adopted by projects; maintaining this repository does not automatically update those projects.

## Versioning conventions
0.x means project-specific adaptation and validation are still required. Patch versions correct wording; minor versions add compatible templates; incompatible workflow changes increment the major version after a stable release. Releases include a CHANGELOG and Git tag. Record unreleased work in progress under Unreleased; do not claim it has been released in advance.
