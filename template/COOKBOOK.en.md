# Development workflow Cookbook for dot

This is a standalone set of instructions you can hand to dot to start work. Its goal is to turn requirements into verifiable, handoff-ready changes, without requiring a particular product, deployment platform, or external review service. This document does not grant authorization or automatically install tools, connect accounts, configure CI, or enable repository protections.

The Chinese and English Cookbooks describe the same workflow and refer to one shared set of bilingual templates. Choose a reading language; do not run the workflow twice or install a second language-specific rule set.

## 1. Copy this to get started

Send this file to dot as an attachment or in full, followed by the text below. Only the target repository and task are required; if you do not have a repository yet, keep “local exercise.”

> Please start work using the attached Cookbook.
> Target repository: local exercise / the repository link or local directory I provide
> Task: run the threshold example in this document first / the specific change I describe
> Work boundaries: local preparation, testing, and delivery only by default; do not push, open a PR, merge, or deploy.
> Optional authorization: perform these two steps only if I explicitly specify the target repository and write “allow pushing the task branch and opening a Draft PR.” Confirm merging, deployment, permissions, credentials, and external review services separately.
> First confirm the target and current environment, read project rules, and provide a short plan. Proceed with work that can safely move forward, asking only genuinely blocking questions. Follow spec → design review → red tests → minimal implementation → green tests and regression checks → independent review → evidence delivery. Explicitly mark steps that cannot run or lack permission as not run/blocked; never fabricate success.

If you receive only this file, without the entire template repository, you can still start with the inline structures in section 4. Do not stop because optional templates are unavailable.

## 2. Cold start: what to read and how to handle gaps

dot should first:
1. Confirm the repository and task supplied by the user. If multiple repositories have the same name, the target branch is unclear, or the task has material ambiguity, ask one minimal clarifying question. Without a project, use the local exercise in section 5; do not create a remote repository.
2. Use the user's specified computer/environment. If none is specified, select an existing environment that can access the target. Verify the path, repository origin, current branch, and working-tree status first. Preserve others' uncommitted changes; do not automatically clean them up, reset them, or include them in this task's commit.
3. Read the target repository's applicable AGENTS.md, README, contribution/run instructions, and relevant specs. Existing project rules take precedence. If they conflict with these instructions or the user's requirements, explain and confirm the conflict rather than silently replacing them.
4. If the full template set is available, read README.en.md, docs/ADOPTION.en.md, and template/docs/workflow-v2.md in that order, then use the spec, acceptance, handoff, and PR templates as needed. Template locations are not fixed locations in the target project.
5. Detect the test commands and tool versions actually available; do not infer them solely from examples. List the independent review mechanism, CI/required checks, merge strategy, and authorized scope. Mark nonexistent project items as “not configured,” never as passed.
6. Provide a short statement of goals, scope, check plan, blockers, and expected deliverables. If login/connection is missing, ask the user to complete it through the product's supported secure method. Do not ask for passwords, tokens, or private keys in chat/files.

The minimum parameters are “where” and “what.” The environment can be detected; test commands come from the project. Without a specified merge strategy, do not merge. Without a specified external review service, do not call one. If independent review is unavailable, preparation and self-review may continue, but independent review must be marked blocked; do not claim readiness to merge.

## 3. Adopt into an existing project (optional, not a prerequisite)

Copy only the templates you need and reconcile them with existing files item by item, without overwriting project rules:
- template/AGENTS.md.template → project AGENTS.md.
- template/docs/workflow-v2.md → project docs/workflow-v2.md.
- template/docs/specs/TEMPLATE.md → project docs/specs/TEMPLATE.md.
- template/docs/ACCEPTANCE_TEMPLATE.md and HANDOFF_TEMPLATE.md → project docs/.
- template/PULL_REQUEST_TEMPLATE.md → a platform-recognized location such as .github/pull_request_template.md; verify other platforms first.

Record the full source commit and adoption date. Pin both a verified tag and commit when available; otherwise pin the commit, without assuming a release already exists. Fill in tests, reviews, checks, and owners. Copying files does not activate platform gates. New Apps, credentials, paid capabilities, repository protections, and permissions require separate confirmation. Adoption changes must also go through project review.

Universal requirements: scope, observable acceptance criteria, actual evidence, independent review, privacy checks, and delivery status.
Project-specific options: CI platform, SAST/lint tools, signing mechanisms, deployment, release, and merge strategy. Once the project requires an item, it is no longer an optional check that can be skipped at will.

## 4. Close the loop on every task

### Roles and gates
- Requirements/design review: check contracts, non-goals, risks, and acceptance criteria.
- Implementer: write tests before implementation and run project checks.
- Independent reviewer: not the implementer's self-review; checks the complete current changes and evidence. This may be another person or an independent internal review session, but the latter is not equivalent to APPROVE from another account on the platform.
- Owner: decides on major changes, merges, and releases. Passing CI replaces none of these approvals.

Even small tasks need a short specification. Obtain explicit approval first for major changes involving permissions, security, irreversible data operations, cross-system refactors, major dependency versions, releases, or deployments. A spec being “written” does not mean it is “approved.”

### Copyable specification
```text
Goal/triggering conditions:
Non-goals:
Inputs, outputs, invariants, error behavior:
Acceptance: requirement → test file/case → input → expected result → validation environment
Design review: conclusion, review source, open questions
Risks and rollback:
Approved scope:
```

Write acceptance tests first. Verify that red means an assertion about the expected behavior failed, not missing dependencies, a mistyped command, or an import failure. When commits are allowed, save the red-test commit before making the smallest fix. When only local preparation is allowed, save patches/logs and do not fabricate SHAs. After green, run relevant checks and the full checks required by the project, distinguishing passed, failed, skipped, not run, and not configured. Documentation-only changes may explain that behavioral red/green testing is not applicable, but must still validate links, examples, and privacy.

Independent review must correspond to the current head. Without a commit, tie it to an explicit file snapshot/checksum and explain the limitation. Subsequent changes require rerunning affected checks and updating the review. Fix failures and review again; do not delete tests, weaken assertions, or skip gates.

### Copyable delivery record
```text
Goal and actual changes:
Environment/tool versions:
base/head SHA (if uncommitted, state that and provide a snapshot identifier):
Acceptance → tests → actual results/log locations:
Cause of red and results of green:
Full checks, CI, and independent review (list separately):
Risks, unverified items, blockers, and next steps:
Committed/pushed/PR opened/merged/released/deployed? (answer separately):
Authorized scope and rollback method:
```

## 5. Runnable recipe: boundary values from red to green

This is an isolated software exercise. It does not operate devices or validate production, and needs no third-party Python packages. Prerequisite: Python 3 is already available. Report its absence rather than installing it automatically. The input contract covers nonnegative integers only; negative numbers and other types are out of scope. Goal: return True when the wait time reaches the threshold.

Create the following two files in a new temporary directory, without overwriting existing project files. Have an independent reviewer confirm this specification and its four cases before running tests. If no independent reviewer is available, you may continue the local exercise and record actual red/green results. Independent review remains blocked; do not claim that every gate is complete.

eligibility.py (deliberately retaining a boundary bug):
```python
def is_eligible(waited_seconds, required_seconds):
    return waited_seconds > required_seconds
```

test_eligibility.py:
```python
import unittest
from eligibility import is_eligible

class BoundaryTests(unittest.TestCase):
    def test_below(self):
        self.assertFalse(is_eligible(9, 10))

    def test_equal(self):
        self.assertTrue(is_eligible(10, 10))

    def test_above(self):
        self.assertTrue(is_eligible(11, 10))

    def test_zero(self):
        self.assertTrue(is_eligible(0, 0))

if __name__ == "__main__":
    unittest.main()
```

Run in that directory:
```sh
python3 -m unittest discover -s . -p 'test_*.py' -v
```

Expected red result: 4 tests, AssertionError in equal and zero, nonzero exit status. After recording the actual results, change only > to >= in eligibility.py, leaving tests unchanged. Run the same command again; all 4 tests are expected to pass. These are expectations only: the dot receiving the Cookbook must run it and report actual results.

Have an independent reviewer check the contract, unchanged tests, and single-operator fix, and rerun the tests independently. This exercise needs no git, network, or CI. If no remote validation was performed, write “CI not run.” If the project needs CI, adapt separately under section 6. Do not treat this example as a general-purpose input-validation library or real hardware acceptance testing.

## 6. CI and PR recipe (requires user authorization)

Once authorized to push to the specified repository and open a PR, use a task branch and Draft PR. Include the specification, acceptance mapping, risks, red/green evidence, and limitations. Do not merge automatically.

Use existing CI workflows when available. If there is no CI, propose a minimal plan first and confirm whether to configure it; do not silently install services. Optional CI for the Python exercise should use an existing trusted runner, a verified Python version, read-only contents permissions, a timeout, no secrets, and no deployment. Pin third-party actions to verified full SHAs; never invent hashes. Do not use privileged pull_request_target on untrusted PR code, or let fork PRs execute on self-hosted runners with access to internal networks/credentials.

When checking, record:
- Exact head SHA, base SHA, every check name, result, run link, and tested commit.
- Push CI can test the original head directly; pull_request CI often tests a synthetic merge commit. Make the distinction explicit.
- Do not erase historical red results; current required checks must pass on the current candidate commit.
- A pass on an old head, a missing check, or a pending status is not current success.
- Record independent analysis separately from formal platform approval. If the platform requires approval by another account, comply; do not fabricate or bypass it.
- Revalidate affected results after CI changes, merge-conflict resolution, or base/head changes.

## 7. Attribution, signatures, and provenance

Check project identity, DCO, and signing rules first. Do not change global Git identity without authorization; confirm conflicts before proceeding. Commit authorship must accurately reflect actual origin: AI implementations use an identifiable AI author name and Agent: <actual-source-identifier>; humans use their chosen identity and Agent: human. Human approval does not turn AI implementation into human implementation. Do not infer origin from email domains or copy real email addresses into document/PR body text.

Authors/trailers are provenance declarations, not cryptographic signatures. The project decides whether GPG/SSH/Sigstore or other signatures are required. If authorized signing capability is unavailable, report a blocker; do not generate/export keys or disable signing requirements. Server-side squash/rebase/merge may alter metadata. Verify the strategy before merging, then reread actual attribution and signature-verification results afterward.

## 8. Privacy, sharing, and synchronization

Share reusable methods, not private context. Export using an explicit file allowlist. Have a human review each file and scan for secrets, real email addresses, account identifiers, internal network addresses, device paths, private logs, and raw chats. Allow example placeholders only; do not embed private project links. Scanning is an aid, not a safety guarantee.

If the source repository contains private material, use a clean export with fresh history. Do not directly push --mirror, copy .git, or make the entire repository public. Public visibility, invitations, and access grants require separate authorization. Others may not be able to read private links; share a reviewed copy of this file or a clean file bundle instead. Granting a license/adding a license is the maintainer's decision; these instructions are not a license.

For multiple repositories, first establish whether they are mirrors sharing history or independent exports. Record the source commit for generic files and synchronize content differences using an allowlist. Preserve repository-specific content, especially without overwriting or exporting private-context. If synchronization direction has not been confirmed, deliver the diff first; do not automatically synchronize both ways.

## 9. Merging, failures, and stop conditions

Merging requires explicit authorization and a project-approved strategy. Immediately before merging, reread remote head/base, PR status, current CI, valid independent review, source attribution, and applicable protections. Stop the action if anything does not comply. Fast-forward merging that preserves original commits is only an optional strategy; do not bypass PRs or protections for attribution.

For denials, protection failures, concurrent updates, unclear permissions, signature mismatches, unknown command side effects, or disclosure risks, report the exact target, reason, and smallest next step. Do not force-push, change protections to get through, or automatically switch accounts to bypass restrictions. If an outcome is uncertain, check status first to avoid duplicate pushes/PRs/deployments.

After an authorized merge, reread the target branch and PR to confirm actual commits and status before reporting. Deployment and release still require separate authorization. Prefer rollback through a new revert change, subject to project checks and approval; do not rewrite history. If only part of the work can be completed, deliver existing patches, specs, evidence, and a list of unfinished items. Do not describe “prepared” work as “live.”
