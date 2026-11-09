# <FEATURE> / <FEATURE>

此文件是唯一共享模板。中英文表达相同规则和字段；填值保持一致，流程只执行一次。两种表述若有实质冲突，先澄清，不自行扩大权限。

This is the single shared template. Both languages express the same rules and fields; keep filled-in values consistent and run the workflow once. Clarify any substantive discrepancy before proceeding; do not expand permissions yourself.

## 简体中文

### <FEATURE>

状态：草案 / 待评审 / 已批准 / 实现中 / 已验收。采用模板：<VERSION>。

#### 背景与目标
<问题、触发条件、可观察目标>

#### 非目标
<本次不处理的范围>

#### 不变量
<契约、安全边界、失败行为>

#### 方案与设计评审
<方案、替代选项、评审人/证据、未决问题>

#### 验收测试清单
- [ ] 需求：<REQUIREMENT>；文件与用例：<TEST>；输入：<INPUT>；预期：<EXPECTED>；验证面：<LOCAL/INTEGRATION/PRODUCTION>。

#### 红绿测试证据
<失败复现 SHA/结果；修复 SHA/结果；不适用时解释原因>

#### 风险与回退
<影响、回退步骤、未验证范围>

#### 是否重大变更
<是/否与理由；见 ../workflow-v2.md 的定义；批准范围与证据>

## English

### <FEATURE>

Status: draft / awaiting review / approved / implementation in progress / accepted. Adopted template: <VERSION>.

#### Background and goal
<Problem, triggering conditions, observable goal>

#### Non-goals
<Scope not addressed in this task>

#### Invariants
<Contracts, security boundaries, failure behavior>

#### Approach and design review
<Approach, alternatives, reviewer/evidence, open questions>

#### Acceptance test checklist
- [ ] Requirement: <REQUIREMENT>; file and case: <TEST>; input: <INPUT>; expected: <EXPECTED>; validation scope: <LOCAL/INTEGRATION/PRODUCTION>.

#### Red/green test evidence
<Failure reproduction SHA/results; fix SHA/results; explain if not applicable>

#### Risks and rollback
<Impact, rollback steps, unverified scope>

#### Is this a major change?
<Yes/no and why; see the definition in ../workflow-v2.md; approval scope and evidence>
