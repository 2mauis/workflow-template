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
- 登记到下一轮（S2/S3，写明级别、来源和对应 head；登记不等于豁免）：<DETAILS_OR_NONE>
- 未验证/跳过/阻塞：<DETAILS>
- 隐私及署名检查：<EVIDENCE>
- 请求合并批准的 head SHA：<HEAD_SHA>；用户批准对应的 head SHA：<APPROVED_HEAD_SHA_OR_NOT_YET>（head 改变后原批准失效，须重新验证并重新取得批准）

#### 发布与部署
<未执行 / 另行授权范围，每次部署单独批准，并写明批准对应的版本（SHA 或 tag）；不把 CI 通过当作部署批准>

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
- Logged for the next round (S2/S3, with severity, source, and the head it applies to; logging does not waive an item): <DETAILS_OR_NONE>
- Unverified/skipped/blocked: <DETAILS>
- Privacy and attribution checks: <EVIDENCE>
- Head SHA for which merge approval is requested: <HEAD_SHA>; head SHA the user approved: <APPROVED_HEAD_SHA_OR_NOT_YET> (if the head changes, the approval lapses; revalidate and obtain approval again)

#### Release and deployment
<Not performed / separately authorized scope, with each deployment approved separately and the approved version (SHA or tag) stated; passing CI is not deployment approval>
