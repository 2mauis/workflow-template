# 协作工作流 v2 / Collaboration workflow v2

此文件是唯一共享模板。中英文表达相同规则和字段；填值保持一致，流程只执行一次。两种表述若有实质冲突，先澄清，不自行扩大权限。

This is the single shared template. Both languages express the same rules and fields; keep filled-in values consistent and run the workflow once. Clarify any substantive discrepancy before proceeding; do not expand permissions yourself.

## 简体中文

### 协作工作流 v2

> 通用基线。项目采用时填写角色、命令、检查与合并策略。本文不授予外部操作权限。

#### 职责分离
需求与架构评审、实现与调试、独立代码评审分开执行。独立评审方式由项目明确配置，不把实现者自查或 CI 通过当作独立评审。

#### 闭环
1. 写清问题、触发条件、目标与非目标；重大变更先写 [spec](specs/TEMPLATE.md)。
2. 先做规格/设计评审，确认验收、风险、回退与批准，再实现。
3. 先提交失败复现或验收测试，再最小修复。失败须为目标行为断言失败，不是安装或导入错误。若禁止提交/推送，只准备变更并记录证据。
4. 运行相关及项目要求的完整检查，逐条记录结果、SHA、环境和未覆盖范围。纯文档变更说明行为测试不适用原因。
5. PR 列验收清单与风险分级，请独立评审。评审必须对应当前 head；head 改变后重新检查受影响范围和评审有效性。
6. 合并前重新读取精确 head/base、PR 状态、所有适用 CI、独立评审及最终署名方案。任一条件不符立即停止。
7. 仅按项目已批准策略合并。遇保护或并发拒绝不得绕过；不得改设置、强推或跳过检查求通过。
8. 合并后回读 main、PR 状态及实际提交署名；事后审计不能替代合并前检查。归档验收、剩余风险与回退方法。

#### Spec 和证据
验收条目映射测试文件、用例名、输入与预期，区分本地/集成/生产。spec 不代替批准。CI、独立评审、合并和部署是不同门禁；本地通过不等于线上通过。失败、阻塞、跳过、未运行分别记录，不把缺失检查写成成功。

#### 重大变更
至少包括权限/安全边界、仓库保护/required checks、凭据、对外发布/部署、数据删除或不可逆迁移、依赖主版本升级及跨多个子系统的大重构。项目可细化阈值；不确定时先确认。批准记录写范围及条件，不保存敏感数据。

#### 署名
AI 实际编写的改动使用可识别的 AI 作者名称及 `Agent: <agent-id>`；人工使用本人选择姓名和邮箱以及 `Agent: human`。AI 来源不以 noreply 判断；人工批准或点击合并不改变 AI 实作来源。不得自动修改账号隐私设置。不要在文档/PR 正文复制真实邮箱。

服务端 squash/rebase/merge 可能重写元数据，必须在合并前核实。若项目批准采用保留原提交的 fast-forward：确认 base 是 head 祖先、全部待合入提交通过署名检查、远端值仍未变化，只用普通非强推更新精确已核验 head；保护拒绝即停止。不把此方案设为所有项目通用默认。

#### 隐私与发布
个人协调信息与项目开发规则分离。公开发布只按经审核的白名单导出，并扫描内容和要发布的历史；不得复制私人仓历史。示例使用占位符和文档保留域名，不写凭据、私人地址或原始线上日志。隐私扫描不能替代人工审查。

#### 项目适配表
- 测试与 lint：<COMMANDS>
- 隐私/secret/SAST：<COMMANDS_OR_UNCONFIGURED>
- 独立评审机制：<REVIEWER_OR_SERVICE>
- required checks：<CHECK_NAMES>
- 合并策略及批准依据：<STRATEGY_AND_APPROVAL>
- 发布与部署：<SEPARATE_AUTHORIZATION_AND_RUNBOOK>

#### 证据与生成文件
观察结果、推导与未知分开记录。模拟测试不代表真实环境验证；标明实际测试条件和样本范围。生成文件应从项目指定源文件再生成，不直接手改生成结果；项目没有此类文件则不适用。

## English

### Collaboration workflow v2

> Generic baseline. Fill in roles, commands, checks, and merge strategy when adopting it for a project. This document does not authorize external actions.

#### Separation of responsibilities
Perform requirements and architecture review, implementation and debugging, and independent code review separately. The project must explicitly configure its independent review mechanism. An implementer's self-review or passing CI does not constitute independent review.

#### End-to-end workflow
1. Define the problem, triggering conditions, goals, and non-goals. Write a [spec](specs/TEMPLATE.md) first for major changes.
2. Review the specification/design first, confirming acceptance criteria, risks, rollback, and approvals before implementing.
3. Commit the failing reproduction or acceptance tests first, then make the smallest fix. The failure must be an assertion about the target behavior, not an installation or import error. If commits/pushes are prohibited, prepare changes and record evidence only.
4. Run relevant checks and all full checks required by the project. Record each result, SHA, environment, and uncovered scope. For documentation-only changes, explain why behavioral tests are not applicable.
5. Include an acceptance checklist and risk level in the PR and request independent review. Review must correspond to the current head; after head changes, recheck affected areas and review validity.
6. Before merging, reread the exact head/base, PR status, all applicable CI, independent review, and final attribution plan. Stop immediately if any condition is unmet.
7. Merge only using the project's approved strategy. Do not bypass protection or concurrent-update rejections; do not change settings, force-push, or skip checks to get through.
8. After merging, reread main, PR status, and actual commit attribution. Post-merge auditing does not replace pre-merge checks. Archive acceptance results, remaining risks, and the rollback method.

#### Specs and evidence
Map acceptance items to test files, case names, inputs, and expected results, distinguishing local/integration/production validation. A spec is not approval. CI, independent review, merging, and deployment are separate gates; passing locally does not mean passing in production. Record failures, blockers, skipped steps, and steps not run separately. Do not report missing checks as successful.

#### Major changes
These include, at minimum, permission/security boundaries, repository protections/required checks, credentials, external releases/deployments, data deletion or irreversible migrations, major dependency upgrades, and large refactors across multiple subsystems. Projects may refine thresholds; ask first when uncertain. Approval records must state scope and conditions without storing sensitive data.

#### Attribution
Changes actually written by AI use an identifiable AI author name and `Agent: <agent-id>`; humans use their chosen name and email address and `Agent: human`. A noreply address does not determine AI origin; human approval or clicking merge does not change the origin of AI implementation. Do not automatically change account privacy settings. Do not copy real email addresses into document/PR body text.

Server-side squash/rebase/merge may rewrite metadata; verify this before merging. If the project approves fast-forward merging to preserve original commits: confirm that base is an ancestor of head, all incoming commits pass attribution checks, and remote values remain unchanged. Use only a normal, non-force push to update to the exact verified head; stop if protections reject it. Do not make this the universal default for all projects.

#### Privacy and release
Keep personal coordination information separate from project development rules. For public releases, export only a reviewed allowlist and scan both contents and the history to be published; do not copy private repository history. Use placeholders and documentation-reserved domains in examples, with no credentials, private addresses, or raw production logs. Privacy scanning does not replace human review.

#### Project configuration
- Tests and lint: <COMMANDS>
- Privacy/secrets/SAST: <COMMANDS_OR_UNCONFIGURED>
- Independent review mechanism: <REVIEWER_OR_SERVICE>
- required checks: <CHECK_NAMES>
- Merge strategy and approval basis: <STRATEGY_AND_APPROVAL>
- Release and deployment: <SEPARATE_AUTHORIZATION_AND_RUNBOOK>

#### Evidence and generated files
Record observations, deductions, and unknowns separately. Simulated tests do not establish validation in a real environment; state the actual test conditions and sample scope. Regenerate generated files from project-designated source files instead of hand-editing the output. This does not apply if the project has no such files.
