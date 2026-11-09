# 验收记录：<TASK> / Acceptance record: <TASK>

此文件是唯一共享模板。中英文表达相同规则和字段；填值保持一致，流程只执行一次。两种表述若有实质冲突，先澄清，不自行扩大权限。

This is the single shared template. Both languages express the same rules and fields; keep filled-in values consistent and run the workflow once. Clarify any substantive discrepancy before proceeding; do not expand permissions yourself.

## 简体中文

### 验收记录：<TASK>

日期/时区：<DATE>。目标提交：<SHA>。环境：<ENVIRONMENT>。

#### 需求与结果
- <需求 → 测试文件/用例 → 通过/失败/跳过/未运行 → 证据>

#### 评审与门禁
- 独立评审及对应 SHA：<EVIDENCE>
- 适用 CI 与结果：<EVIDENCE>
- 批准与范围：<EVIDENCE>
- 合并前后 SHA/PR 状态/署名：<EVIDENCE_OR_NOT_MERGED>

#### 限制与后续
<本地不代表生产；剩余风险、负责人、下一步；回退方法>

## English

### Acceptance record: <TASK>

Date/time zone: <DATE>. Target commit: <SHA>. Environment: <ENVIRONMENT>.

#### Requirements and results
- <Requirement → test file/case → passed/failed/skipped/not run → evidence>

#### Reviews and gates
- Independent review and corresponding SHA: <EVIDENCE>
- Applicable CI and results: <EVIDENCE>
- Approvals and scope: <EVIDENCE>
- Pre-/post-merge SHA/PR status/attribution: <EVIDENCE_OR_NOT_MERGED>

#### Limitations and follow-up
<Local results do not establish production results; remaining risks, owner, next steps; rollback method>
