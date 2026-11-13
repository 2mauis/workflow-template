# Adoption and upgrades

Start with the [standalone Cookbook you can hand to dot](../template/COOKBOOK.en.md). If you only want to start a task, you do not need to install every template first.

The Chinese and English adoption guides both point to the same set of bilingual shared templates in `template/`. Each field has exactly one meaning and value; keep the two languages consistent, and do not adopt two sets of rules.

The current workflow version is v2.1. The rules file is still named `template/docs/workflow-v2.md`; the version is given by the file title and the "Change log" at its end.

## First adoption
1. If a verified release tag exists, record the tag and its full commit; if there is no tag, pin the full commit and note that it is unreleased. Do not rely on a movable main.
2. Copy only the files you need from `template/` into the project. Do not copy private-context or this repository's history.
3. Save `AGENTS.md.template` as the project's AGENTS.md. Put the PR template where the target platform recognizes it (for example `.github/pull_request_template.md` or `.gitea/pull_request_template.md`), and verify platform support before adopting it.
4. Fill in project commands, test paths, the independent review mechanism, coding tool and tier, technical research role, attribution trailer source, required checks, release boundaries, and merge strategy. The default tools and models in the workflow can be overridden in the configuration table. If the project has not configured the following two items, do not write "not configured"; enter the default instead:
   - Technical research role: the agent responsible for coding checks the official documentation itself and attaches source links.
   - Attribution trailer source: if there is no attribution script or policy file, add source trailers depending on where the coding agent runs. In the cloud (for example Codex cloud), record only the tool identifier, such as `Code-Tool: codex <model> <tier>`, with no Host-Id; on a local computer, record the same tool identifier and add `Host-Id: <host-id>`. Documents use only placeholders, never a real host name. Before adopting this: for local runs, the host id appears in the public commit history.

   Mark any other nonexistent checks as "not configured", never as passed.
5. Merge the adoption only after the project's own review and approval, and record the template source version. Copying files does not put the process into effect.
6. Validate with a small task first: failure reproduction → implementation → regression → independent review → exact-SHA check. Release and deployment follow the project's separate authorization.

## Configurable items and distinctions
- Approvals, protections, checks, and review apps differ between GitHub and Gitea; do not assume they are equivalent.
- Do not install apps, create tokens, or change permissions or required checks by default.
- Fast-forward merging that preserves original commits is an optional strategy, not the default for every project. If protection requirements are not met, stop; do not switch to a workaround.
- Documentation-only changes may state that behavioral tests are not applicable and explain why; do not fabricate red/green evidence.
- Private staging must not mix in personal coordination either. Releases use the project's own explicit allowlist and privacy checks, without private history.

## Upgrade and rollback
Upgrade through a separate PR that compares two pinned commits (with tags, if any). Carry over only applicable rules, and record conflicts and the reasons for each adoption decision. Roll back with a new revert commit that restores the previous verified version; do not force-push or delete history. Record the template version and each project's adopted version separately; maintaining this repository does not update adopting projects automatically.

When upgrading from v2 to v2.1, also check the following:
- Whether the three new configuration rows (coding tool and tier, technical research role, attribution trailer source) are filled in, and whether the last two use the defaults where the project has not configured them.
- Before running on a local computer with the Host-Id trailer, whether the project is aware that the host id appears in the public commit history.
- Whether the default roles fit this project; if not, override them in the configuration table.
- Whether new rules such as the stop-point list and separate scan and push conflict with the project's existing gates; confirm any conflict rather than silently replacing existing rules.

## Versioning
0.x means the templates still need to be adapted and validated in each project. Patch versions fix wording; minor versions add compatible templates; incompatible process changes bump the major version once a stable version exists. Released versions come with a CHANGELOG entry and a Git tag; unreleased changes under development are recorded as Unreleased and are not described as released ahead of time.

The template repository version (such as 0.2.0) and the workflow version (such as v2.1) are numbered separately: the former identifies a release of the whole template set, the latter the workflow rules themselves, recorded in the title and change log of `workflow-v2.md`.
