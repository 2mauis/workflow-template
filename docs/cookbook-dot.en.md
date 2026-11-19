# Development workflow Cookbook for Dot (workflow v2.2)

Language: **English** · [简体中文](cookbook-dot.md)

This is a standalone set of instructions you can hand to Dot (dots in ChatGPT) to start work. Its goal is to turn requirements into verifiable, handoff-ready changes, without requiring a particular deployment platform or external review service. This document does not grant authorization or automatically install tools, connect accounts, configure CI, or enable repository protections.

This Cookbook matches workflow v2.2 and ships with template repository 2.2.2. The rules file is template/docs/workflow-v2.md; its name does not change with minor versions, and the version is given by the file title and the "Change log" at its end.

A separate [Cookbook for Grok Bot](cookbook-grok-bot.en.md) describes the same rules applied to multi-bot collaboration. Both have the same stop points and approval red lines; pick the one that matches the implementer and do not apply both. The Chinese and English Cookbooks describe the same workflow and refer to one shared set of bilingual templates. Choose a reading language; do not run the workflow twice or install a second language-specific rule set.

## 0. Verified Dot capabilities (with sources)

Verified on 2026-11-15. Only capabilities that can be verified on official OpenAI pages are listed; anything that could not be found is left out. The product changes, so check the official pages before relying on them. This is a product description, not an authorization: whether a capability may be used in this workflow is still governed by the work boundaries in section 1 and the stop points in section 4.

- **D1 Cloud computer and browser.** Dot has its own computer and browser in the cloud and can keep working when the user's computer is off. Sources: [Dots](https://learn.chatgpt.com/docs/dots), [Connect computers and apps](https://learn.chatgpt.com/codex/dots/computers-and-apps)
- **D2 Kinds of work.** It can research, analyze data, prepare documents, and build software, using past conversations and preferences as context. Source: [Dots](https://learn.chatgpt.com/docs/dots)
- **D3 Background agents.** It can divide work among background agents that run in parallel and report back to it, while the user keeps talking to Dot. Source: [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory)
- **D4 Delegation and Codex cloud environments.** It can delegate parts of a request to ChatGPT Work or Codex; coding work that needs a particular repository and setup can use a Codex cloud environment the user has created. Sources: [Get started](https://learn.chatgpt.com/codex/dots/getting-started), [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory)
- **D5 Connecting the user's computer.** Once connected, Dot can use that computer's local files, code, and apps; the computer must stay online with the ChatGPT app open while in use; each thread uses one computer at a time; changing the selected computer does not move an existing task; connecting a computer does not grant access to every existing ChatGPT or Codex conversation. Sources: [Connect computers and apps](https://learn.chatgpt.com/codex/dots/computers-and-apps), [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory)
- **D6 Plugins.** It can use enabled plugins (such as GitHub); access depends on the plugin's connected account, its permissions, and the environment where the work runs; the user's existing ChatGPT app permissions apply to Dot. Sources: [Dots](https://learn.chatgpt.com/docs/dots), [Connect computers and apps](https://learn.chatgpt.com/codex/dots/computers-and-apps)
- **D7 Website sign-in.** When a website needs a login, Dot sends a private form, and the information goes to the remote browser outside the conversation; the user can also take over the cloud browser, do a step, and return control; reusing a saved login requires the user's confirmation. Sources: [Connect computers and apps](https://learn.chatgpt.com/codex/dots/computers-and-apps), [Dots](https://learn.chatgpt.com/docs/dots)
- **D8 Automatic review.** Before Dot takes an action that could affect the user's accounts or share information, an automatic review decides whether the action can proceed, needs the user's approval, or includes a step the user must do; asking Dot to draft replies does not give it permission to send them. Source: [Control your dot](https://learn.chatgpt.com/codex/dots/controls)
- **D9 Custom instructions and custom rules.** Rules can be "Ask before taking action" or "Hand off to you"; rules are instructions Dot tries to follow and can get wrong, they do not grant access, do not override built-in safety requirements, and do not remove required confirmations such as approval to reuse a saved login. Source: [Control your dot](https://learn.chatgpt.com/codex/dots/controls)
- **D10 Pausing and stopping.** Pause stops only the current main task; it does not stop delegated tasks or cancel scheduled runs; stopping does not undo completed actions; delegated tasks are stopped in Activity and recurring tasks in Scheduled. Sources: [Control your dot](https://learn.chatgpt.com/codex/dots/controls), [Dots](https://learn.chatgpt.com/docs/dots)
- **D11 Notes.** Dot keeps notes about preferences, decisions, and ongoing work, used across conversations and contact methods; these notes are separate from ChatGPT's saved memory and are not a complete transcript. Source: [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory)
- **D12 Channels.** Dot can be reached in ChatGPT (mobile app, desktop app, desktop browser; mobile web is not supported), Slack, and Microsoft Teams; texting is in a limited beta. Connecting a messaging channel does not grant access to an inbox, other apps, or a computer; before sharing information from a private conversation with others in a channel, Dot checks that the user has allowed it. Sources: [Message your dot](https://learn.chatgpt.com/codex/dots/channels), [Get started](https://learn.chatgpt.com/codex/dots/getting-started)
- **D13 Schedules.** Work that repeats at fixed times needs a saved schedule; stopping active work and canceling a schedule are separate actions. Source: [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory)
- **D14 Results need checking.** A completed run does not by itself confirm that the requested result was achieved or delivered; outputs can contain mistakes, so important results should be reviewed. Sources: [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory), [Dots](https://learn.chatgpt.com/docs/dots)
- **D15 Research is separate from action.** Research itself is read-only: it does not send messages, change connected apps, or control a browser or computer; any follow-up action is subject to the same permissions and safety requirements. Sources: [Tasks and memory](https://learn.chatgpt.com/codex/dots/tasks-and-memory), [Control your dot](https://learn.chatgpt.com/codex/dots/controls)

Availability is rolling out by account type, region, and workspace eligibility; see [Dots](https://learn.chatgpt.com/docs/dots). This document does not cover plans or usage limits.

Not verified (and therefore not written into this document):
- The OpenAI announcement page <https://openai.com/index/introducing-dots> returned 403 to automated fetching at verification time, so its content could not be verified. Details reported by third parties, such as the underlying model name or the number of connectable apps, are not included.
- The official pages do not say how to view, edit, or delete Dot's notes.
- Calls initiated by Dot: the official page describes these as planned for after launch, so they are not treated as an existing capability.

### Dot-specific red lines (tightening only, never loosening)
- Automatic review letting an action proceed (D8), custom rules (D9), and conclusions from background agents (D3) or delegated tasks (D4) are not user approval. Every stop point in section 4 waits for the user's explicit approval in person.
- Writing the stop points in section 4 as "Ask before taking action" rules is recommended as an aid; rules can be wrong (D9), so Dot still checks the stop-point list itself before every action. Do not write any rule that lets a stop-point action proceed automatically.
- Drafting is not sending (D8): PR comments, messages, email, and other outward-facing content are sent only within the authorized scope.
- Credentials go only through the private form or a user takeover (D7) and never appear in the conversation, files, or task briefs; do not reuse a saved login to skip a confirmation.
- Stop means stop everything: when the user asks to stop, pause the main task, stop delegated tasks in Activity, cancel the related schedules in Scheduled, and report truthfully which completed actions cannot be undone (D10, D13).
- A finished run is not a finished delivery (D14): delivery includes the actual evidence required in section 4.

## 1. Copy this to get started

Send this file to Dot as an attachment or in full (in ChatGPT, Slack, or Teams; see D12), followed by the text below. Only the target repository and task are required; if you do not have a repository yet, keep “local exercise.”

> Please start work using the attached Cookbook (workflow v2.2).
> Target repository: local exercise / the repository link or local directory I provide
> Task: run the threshold example in this document first / the specific change I describe
> Work boundaries: local preparation, testing, and delivery only by default; do not push, open a PR, merge, or deploy.
> Optional authorization: perform these two steps only if I explicitly specify the target repository and write “allow pushing the task branch and opening a Draft PR.” Confirm merging, deployment, permissions, credentials, and external review services separately.
> Stop points: at any stop point listed in section 4, stop and wait for my approval in person; your automatic review, custom rules, or background agents' conclusions are not my approval.
> First confirm the target and current environment, read project rules, list every permission the end-to-end task needs in one batch, and provide a short plan. Proceed with work that can safely move forward; look up technical questions in official documentation, with links, and ask only genuinely blocking questions. Follow spec → design review → red tests → minimal implementation → green tests and regression checks → independent review (graded S1/S2/S3: rework S1 issues and S2 issues that block a merge or the main flow in this round, and log the rest for the next round) → evidence delivery. Explicitly mark steps that cannot run or lack permission as not run/blocked; never fabricate success.

If you receive only this file, without the entire template repository, you can still start with the inline structures in section 4. Do not stop because optional templates are unavailable.

## 2. Cold start: what to read and how to handle gaps

Dot should first:
1. Confirm the repository and task supplied by the user. If multiple repositories have the same name, the target branch is unclear, or the task has material ambiguity, ask one minimal clarifying question. Without a project, use the local exercise in section 5; do not create a remote repository.
2. Use the user's specified computer/environment. If none is specified, select an existing environment that can access the target. Dot works on its own cloud computer by default (D1); when local files, code, or signed-in sessions are needed, use a computer the user has connected, and confirm it is online with the ChatGPT app open (D5). Verify the path, repository origin, current branch, and working-tree status first. Preserve others' uncommitted changes; do not automatically clean them up, reset them, or include them in this task's commit.
3. Read the target repository's applicable AGENTS.md, README, contribution/run instructions, and relevant specs. Existing project rules take precedence. If they conflict with these instructions or the user's requirements, explain and confirm the conflict rather than silently replacing them. If an older preference in Dot's notes (D11) conflicts with the repository rules, the repository wins.
4. If the full template set is available, read README.en.md, docs/ADOPTION.en.md, and template/docs/workflow-v2.md (workflow v2.2) in that order, then use the spec, acceptance, handoff, and PR templates as needed. Template locations are not fixed locations in the target project.
5. Detect the test commands and tool versions actually available; do not infer them solely from examples. List the independent review mechanism, coding tool and tier, technical research role, attribution trailer source, CI/required checks, merge strategy, and authorized scope. Mark nonexistent project items as “not configured,” never as passed. The technical research role and the attribution trailer source are exceptions: if the project has not configured them, follow the defaults in sections 4 and 7 instead of writing “not configured.” If the project specifies a model and tier, confirm that they can be selected before starting design, review, or coding; if not, stop and report, and do not switch models or lower the tier on your own.
6. Provide a short statement of goals, scope, check plan, blockers, and expected deliverables, and list every permission the end-to-end task needs in one batch. If login/connection is missing, or the user must authorize something in person, list the concrete steps immediately and ask the user to complete them through the product's supported secure method (for website sign-in, the private form or a user takeover of the browser; see D7); do not shelve the task while waiting. Do not ask for passwords, tokens, or private keys in chat/files.

The minimum parameters are “where” and “what.” The environment can be detected; test commands come from the project. Without a specified merge strategy, do not merge. Without a specified external review service, do not call one. If independent review is unavailable, preparation and self-review may continue, but independent review must be marked blocked; do not claim readiness to merge. The workflow v2.2 default roles (Claude for design and review, Codex cloud for coding) apply only when the project has adopted them and the tools are available; this Cookbook does not require them.

Configurable environment default: prefer the cloud computer. Follow the user’s task-specific environment choice; when work depends on local files, apps or signed-in sessions, select a suitable environment with authorized access to those resources. Verify the current environment and target before acting. Prefer an existing connected API/connector that supports the operation (for Dot, an enabled plugin; see D6), and use read-only checks to establish existing repository access and concrete capabilities; do not request duplicate authorization merely because a CLI is not logged in. Distinguish unsupported capabilities from denied access; never switch environments, accounts or access paths to evade a denial or security restriction.

Before starting, inventory every permission the end-to-end task needs (repositories, branches, plugins, connected computers, cloud environments, review services, and so on) and request them from the user in one batch, rather than asking step by step. A messaging channel is not a permission (D12). Keys and credentials never land on your work machine; steps that need credentials run on the user's own computer or through an authorized connector.

## 3. Adopt into an existing project (optional, not a prerequisite)

Copy only the templates you need and reconcile them with existing files item by item, without overwriting project rules:
- template/AGENTS.md.template → project AGENTS.md.
- template/docs/workflow-v2.md → project docs/workflow-v2.md (same file name; the content is v2.2).
- template/docs/specs/TEMPLATE.md → project docs/specs/TEMPLATE.md.
- template/docs/ACCEPTANCE_TEMPLATE.md and HANDOFF_TEMPLATE.md → project docs/.
- template/PULL_REQUEST_TEMPLATE.md → a platform-recognized location such as .github/pull_request_template.md; verify other platforms first.

This Cookbook is a set of instructions shared with the implementer, not a template to copy into the project.

Record the full source commit and adoption date. Pin both a verified tag and commit when available; otherwise pin the commit, without assuming a release already exists. Fill in tests, reviews, checks, and owners, plus the configuration rows for coding tool and tier, technical research role, and attribution trailer source (added in v2.1 and still used in v2.2), and the next-round log location (added in v2.2). If the project has no technical research role, enter “the agent responsible for coding checks the official documentation itself and attaches source links”. If it has no attribution script or policy file, enter “`Code-Tool: <tool> <model> <tier>` for both cloud and local runs.” If no next-round log location is designated, enter “comments on the relevant PR and the handoff record.” Copying files does not activate platform gates. New Apps, plugins, credentials, paid capabilities, repository protections, and permissions require separate confirmation. Adoption changes must also go through project review.

Universal requirements: scope, observable acceptance criteria, actual evidence, independent review, privacy checks, and delivery status.
Project-specific options: CI platform, SAST/lint tools, signing mechanisms, deployment, release, and merge strategy. Once the project requires an item, it is no longer an optional check that can be skipped at will.

## 4. Close the loop on every task

### Roles and gates
- Requirements/design review: check contracts, non-goals, risks, and acceptance criteria.
- Implementer: write tests before implementation, run project checks, and rework according to the review's severity.
- Independent reviewer: not the implementer's self-review or an agent's self-assessment; checks the complete current changes and evidence and gives an S1/S2/S3 decision. This may be another person or an independent internal review session, but the latter is not equivalent to APPROVE from another account on the platform. Check results from Dot's own background agents (D3) or from tasks it delegated (D4) are self-review on the implementation side, not independent review.
- Technical research: questions about technical approaches, API usage, official documentation, or platform rules are answered from the official source, with links, before anything is implemented or decided. Do not answer from memory, and do not hand such questions back to the user. If the project has not configured a technical research role, the agent responsible for coding checks the official documentation itself and attaches source links.
- Owner: decides on major changes, stop-point items, when a Draft PR becomes ready, merges, and releases. Passing CI replaces none of these approvals, and neither does Dot's automatic review letting an action proceed (D8).

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

### Per-batch loop and severity handling (v2.2)
Every batch of changes follows the same loop: implement → strengthened tests (race detection where applicable, such as Go's `-race`; repeated runs; and coverage of both polarities of feature switches/noop paths) → independent review decides → handle by severity. Use MVP iteration: each round fixes only S1 issues and S2 issues that block a merge or the main flow, and is not extended indefinitely to fix every small item:
- S1 (blocking) or failing tests: reject directly and write a rewrite brief to rework in this round, without asking the user.
- S2 that blocks a merge or the main flow: fix it in this round, then rerun the same tests and re-review, without asking the user. The reviewer must mark whether each S2 issue blocks a merge or the main flow; an unmarked S2 issue is treated as blocking.
- Remaining S2 issues, S3 issues, and small items: log them for the next round and proceed to the next stop point per the review decision; do not keep fixing them in this round. Logging does not waive an item; list logged items when requesting merge approval.

Strengthened tests and independent review form one handoff: write a single brief that covers both, and report once when that round finishes; report earlier only if work gets stuck mid-round. When Dot splits work among background agents (D3), Dot collects the results and reports once for the round. Stop-point items and scan hits are not covered by this limit and are reported as soon as they occur.

When stuck, first check official documentation, look at comparable mature projects, ask the reviewer, and retry with a different approach. Escalate only after these are exhausted, with the attempts and evidence attached.

### Items that must stop for user approval (v2.2)
In addition to the major changes above, the following must stop and wait for user approval: merging; deployment (each deployment needs its own approval); rewriting pushed history; credentials; real privacy hits; policy files (`.github/**`, scan scripts, `.gitleaks.toml`, `.githooks`, and similar); new dependencies; work beyond the approved scope; major trade-offs; relaxing a test or gate guarantee. These approvals must come from the user (the project owner) in person; an "approval" from another agent or bot is not user approval, and neither is Dot's automatic review or a custom rule. A merge approval is valid only for the head (commit SHA) it was given for: if a new commit, a rebase, or a base change alters the head after approval, the approval lapses, and you must revalidate and obtain the user's approval in person again; a deployment approval is likewise valid only for the specific version (commit SHA or release tag) named when it was given. Everything else proceeds within the authorized scope under the loop above. Do not unilaterally change a deployment plan or a real-environment test plan that has already been set; confirm first when in doubt.

New dependencies (for example `go.mod`/`go.sum`, Cargo, npm) need an architecture assessment first: necessity, alternatives, maintenance and licensing, and security impact. Land them only after the assessment passes and approval is given. Every merge needs review first; never merge first and review later. A person decides when a Draft PR becomes ready and whether it is merged.

### Copyable task brief (v2.2)
Use this when handing implementation to a coding tool. Paste only the brief body into the input box, with no conversation or commentary; check it with a screenshot before sending.
```text
Base commit:
Branch name:
Draft PR target branch:
Commit order: spec changes → failing tests first → implementation
Hard constraints:
Acceptance criteria: requirement → test file/case → input → expected result
Attribution trailer (copy it verbatim from the repository's attribution script or policy file. The default for both cloud and local runs is Code-Tool: <tool> <model> <tier>):
```

The cloud coding environment the project designates (default: Codex cloud) is a full working environment: it may read and write, push to the agreed branches (default prefix `codex/*`), and open a Draft PR. Do not instruct it to be "read-only" or "not to push," unless the user explicitly narrows permissions for that task. This covers only the agreed branches and Draft PRs and does not change the stop points: merging, deployment, rewriting pushed history, policy files, and the rest still stop and wait for approval under the list above. To retrieve changes, prefer having it push the agreed branch and open a Draft PR, provided that pushing and opening a PR have been authorized. Without that authorization, the default boundary in section 1 still applies: prepare locally only.

When Dot delegates coding to Codex (D4), use a Codex cloud environment the user has created whose Git hosting connection is authorized for this repository, and fill in the task body using the brief above. Before submitting a cloud task, verify that the environment is cloud, not local, that the model and tier are correct, and that the environment's Git hosting connection is authorized for this repository. If the task entry point has no branch picker, state the base branch in the task body. After submission, watch it through to completion and report the result proactively; a task shown as complete does not mean the result was achieved (D14), so retrieve the changes and evidence and check them before reporting. On an authorization denial or an unavailable branch, retry and actually verify the current authorization and branch state; do not reuse an earlier failure conclusion, and do not shelve the task on that basis.

### Copyable delivery record
```text
Goal and actual changes:
Environment/tool versions (including model and tier; state separately Dot's cloud computer, a connected computer, or a Codex cloud environment):
base/head SHA (if uncommitted, state that and provide a snapshot identifier):
Acceptance → tests → actual results/log locations:
Cause of red and results of green:
Strengthened tests (race, repeated runs, both switch polarities):
Full checks, CI, and independent review (list separately):
Review severity (S1/S2/S3) and how it was handled:
S2/S3 items logged for the next round (use the "Next-round log location" in the configuration table; if none is set, write them in comments on the relevant PR and in the handoff record; do not change repository settings in order to log them):
Privacy and credential scan command and result (separate from the push command):
Risks, unverified items, blockers, and next steps:
Committed/pushed/PR opened/merged/released/deployed? (answer separately):
Head SHA the merge approval covers, and version the deployment approval covers (write "not approved" if there is none):
Delegated tasks and schedules still running (write "none" if there are none):
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

Expected red result: 4 tests, AssertionError in equal and zero, nonzero exit status. After recording the actual results, change only > to >= in eligibility.py, leaving tests unchanged. Run the same command again; all 4 tests are expected to pass. These are expectations only: the Dot receiving the Cookbook must run it and report actual results.

Have an independent reviewer check the contract, unchanged tests, and single-operator fix, and rerun the tests independently. This exercise needs no git, network, or CI. If no remote validation was performed, write “CI not run.” If the project needs CI, adapt separately under section 6. Do not treat this example as a general-purpose input-validation library or real hardware acceptance testing.

## 6. CI and PR recipe (requires user authorization)

Once authorized to push to the specified repository and open a PR, use a task branch and Draft PR. Include the specification, acceptance mapping, risks, red/green evidence, and limitations. Do not merge automatically; a person decides when the Draft PR becomes ready.

Before pushing, run the privacy and credential scan as its own command, then push with a separate command; never combine the two in one command. The check covers at least personal information, host names, internal domains, email addresses (except noreply and documentation-reserved domains), IP addresses, tokens, and keys, across the changes to be pushed, the commit messages, and the PR body. If the scan reports any hit (including one you believe is a false positive), stop and report it first; do not decide on your own and push, and do not add the hit to an allowlist, ignore rule, or baseline on your own.

Use existing CI workflows when available. If there is no CI, propose a minimal plan first and confirm whether to configure it; changes to policy files such as `.github/**` are a stop point that requires user approval, so do not silently install services. Optional CI for the Python exercise should use an existing trusted runner, a verified Python version, read-only contents permissions, a timeout, no secrets, and no deployment. Pin third-party actions to verified full SHAs; never invent hashes. Do not use privileged pull_request_target on untrusted PR code, or let fork PRs execute on self-hosted runners with access to internal networks/credentials.

When checking, record:
- Exact head SHA, base SHA, every check name, result, run link, and tested commit.
- Push CI can test the original head directly; pull_request CI often tests a synthetic merge commit. Make the distinction explicit.
- Do not erase historical red results; current required checks must pass on the current candidate commit.
- A pass on an old head, a missing check, or a pending status is not current success.
- Record independent analysis separately from formal platform approval. If the platform requires approval by another account, comply; do not fabricate or bypass it.
- Revalidate affected results after CI changes, merge-conflict resolution, or base/head changes.

## 7. Attribution, signatures, and provenance

First use read-only checks to verify existing access to the target repository and the capabilities of the current tools. A CLI without a login does not mean a connector or plugin lacks access; do not request duplicate authorization on that basis. Preserve and disclose Git author/committer metadata truthfully, without impersonation; use an identifiable AI author name when the tool supports it and project rules allow it. If the tool cannot set an AI author and project rules permit the fallback, retain the connected-account identity actually set by the tool, explicitly state AI authored in the commit message, add `Agent: <actual-source-identifier>`, and disclose AI origin, the actual author/committer and the tool limitation in the PR. Do not claim that the author field is an AI identity. If mandatory AI-author, DCO or signing rules cannot be met through the current path, stop the affected commit/publication and ask for direction; do not bypass requirements, change rules or create credentials on your own. Take the exact trailer keys and format from the project's attribution script or policy file verbatim; do not write them from memory.

If the project has no attribution script or policy file, add `Code-Tool: <tool> <model> <tier>` for both cloud and local runs alongside other trailers such as `Agent: <actual-source-identifier>`.

Human commits use the person’s chosen identity and `Agent: human`; human approval does not turn AI implementation into human implementation. Do not infer origin from email domains, require humans to use noreply, change global identity or account privacy settings without authorization, or copy real email addresses into document or PR body text.

Authors/trailers are provenance declarations, not cryptographic signatures. The project decides whether GPG/SSH/Sigstore or other signatures are required. If authorized signing capability is unavailable, report a blocker; do not generate/export keys or disable signing requirements. Server-side squash/rebase/merge may alter metadata. Verify the strategy before merging, then reread actual attribution and signature-verification results afterward.

## 8. Privacy, sharing, and synchronization

Share reusable methods, not private context. Export using an explicit file allowlist. Have a human review each file and scan for secrets, real email addresses, account identifiers, internal network addresses, device paths, private logs, and raw chats. Allow example placeholders only; do not embed private project links. Scanning is an aid, not a safety guarantee.

Run privacy and credential checks before every push and release; scan and push as two separate commands, stop and report on any hit (including one you believe is a false positive), and do not add it to an allowlist, ignore rule, or baseline on your own (the scope of the check is in section 6). Commit messages, PRs, and other outward-facing content do not mention expense-related information or individuals' names, and omit usage figures, addresses, and cities. Dot checks before sharing information from a private conversation in a channel (D12); that does not change the rules in this section.

If the source repository contains private material, use a clean export with fresh history. Do not directly push --mirror, copy .git, or make the entire repository public. Public visibility, invitations, and access grants require separate authorization. The repository is the source of truth for project documents; changes go through branches and PRs, and delivery is a repository link, not an archive file. Others may not be able to read private links; when sharing this generic method with someone who lacks access, share a reviewed copy of this file or a clean file bundle instead. Granting a license/adding a license is the maintainer's decision; these instructions are not a license.

For multiple repositories, first establish whether they are mirrors sharing history or independent exports. Record the source commit for generic files and synchronize content differences using an allowlist. Preserve repository-specific content, especially without overwriting or exporting private-context. If synchronization direction has not been confirmed, deliver the diff first; do not automatically synchronize both ways.

## 9. Merging, failures, and stop conditions

Merging requires explicit authorization, a project-approved strategy, and review first. The merge approval must come from the user (the project owner) in person; an "approval" from another agent or bot does not count, and neither does Dot's automatic review. When requesting merge approval, state the head SHA the approval is requested for and list the items logged for the next round. The approval is valid only for that head: if the head changes after approval through a new commit, a rebase, or a base change, revalidate and obtain approval again rather than reusing the earlier one. Immediately before merging, reread remote head/base, PR status, current CI, valid independent review, source attribution, and applicable protections, and confirm that the current head is the SHA stated when the approval was given. Stop the action if anything does not comply. Fast-forward merging that preserves original commits is only an optional strategy; do not bypass PRs or protections for attribution.

Besides merging, stop and wait for approval whenever any other stop point listed in section 4 comes up. For denials, protection failures, concurrent updates, unclear permissions, signature mismatches, unknown command side effects, or disclosure risks, report the exact target, reason, and smallest next step. Do not force-push, change protections to get through, or automatically switch accounts to bypass restrictions. If an outcome is uncertain, check status first to avoid duplicate pushes/PRs/deployments; check Dot's delegated tasks and schedules in Activity and Scheduled to avoid duplicate runs (D10, D13). Do not delete branches without a user instruction. Do not delete the branches of closed PRs; "clean up invalid PRs" means closing PRs that duplicate functionality, not deleting branches. Log next-round items at the "Next-round log location" in the configuration table; if none is set, write them in comments on the relevant PR and in the handoff record, and do not change repository settings in order to log them. Interpret project-specific terms as the project defines them; if the meaning is unclear and would affect direction, confirm first. When the user must sign in or authorize in person, list the concrete steps immediately.

After an authorized merge, reread the target branch and PR to confirm actual commits and status before reporting. Deployment and release still require separate authorization; each deployment needs its own approval, and a previous deployment approval is not reused. The approval is valid only for the specific version it names; if the version changes, obtain approval again. Prefer rollback through a new revert change, subject to project checks and approval; do not rewrite history. If only part of the work can be completed, deliver existing patches, specs, evidence, and a list of unfinished items. Do not describe “prepared” work as “live.”

## 10. Retrospectives and reporting (v2.2)

- After each task, report the result and state whether there is a next step; the delivery record in section 4 can be used directly. When strengthened tests and independent review form one handoff, report once at the end of that round, and report earlier only if work gets stuck mid-round; stop-point items and scan hits are not covered by this limit and are reported as soon as they occur.
- When a run goes badly or the user points out a problem, run a retrospective proactively: record the lesson in agent memory or a skill (for Dot, its notes; see D11), and, if the project has adopted the workflow, update its change log through a branch and PR. Notes do not replace the rules and handoff records in the repository. Base the retrospective practice on official documentation and mature open-source projects.
