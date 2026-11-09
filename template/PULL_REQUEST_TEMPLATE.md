#  目标与范围 /  Goal and scope

此文件是唯一共享模板。中英文表达相同规则和字段；填值保持一致，流程只执行一次。两种表述若有实质冲突，先澄清，不自行扩大权限。

This is the single shared template. Both languages express the same rules and fields; keep filled-in values consistent and run the workflow once. Clarify any substantive discrepancy before proceeding; do not expand permissions yourself.

## 简体中文

#### 目标与范围
<问题、方案、非目标；spec 路径>

#### 验收清单
- [ ] <需求 → 测试/用例 → 真实结果与证据>

#### 风险分级
<低/中/高及原因；回退方式；重大变更批准>

#### 验证
- head/base：<SHA>
- 红绿证据：<RESULT_OR_JUSTIFIED_NA>
- CI：<CHECKS_AND_RESULT>
- 独立评审：<CURRENT_HEAD_REVIEW>
- 未验证/跳过/阻塞：<DETAILS>
- 隐私及署名检查：<EVIDENCE>

#### 发布与部署
<未执行 / 另行授权范围；不把 CI 通过当作部署批准>

## English

#### Goal and scope
<Problem, approach, non-goals; spec path>

#### Acceptance checklist
- [ ] <Requirement → test/case → actual result and evidence>

#### Risk level
<Low/medium/high and why; rollback method; major-change approval>

#### Validation
- head/base: <SHA>
- Red/green evidence: <RESULT_OR_JUSTIFIED_NA>
- CI: <CHECKS_AND_RESULT>
- Independent review: <CURRENT_HEAD_REVIEW>
- Unverified/skipped/blocked: <DETAILS>
- Privacy and attribution checks: <EVIDENCE>

#### Release and deployment
<Not performed / separately authorized scope; passing CI is not deployment approval>
