# 协作工作流 v2.2 / Collaboration workflow v2.2

此文件是唯一共享模板。中英文表达相同规则和字段；填值保持一致，流程只执行一次。两种表述若有实质冲突，先澄清，不自行扩大权限。

This is the single shared template. Both languages express the same rules and fields; keep filled-in values consistent and run the workflow once. Clarify any substantive discrepancy before proceeding; do not expand permissions yourself.

## 简体中文

### 协作工作流 v2.2

> 通用基线。项目采用时填写角色、命令、检查与合并策略。本文不授予外部操作权限；文中的“自动推进”只在已授权范围内适用。文中的工具与模型均为默认值，项目可在适配表中覆盖。文中“用户”指为本任务授权的人（通常是项目负责人）。版本变化见文末「变更记录」。

#### 环境与已有授权
可配置环境默认：云电脑优先。用户为任务指定环境时遵从该选择；任务依赖本地文件、应用或已登录会话时选择能合法访问这些资源的合适环境。操作前先核实当前环境及目标，优先使用已连接且支持所需操作的 API/连接器，并以只读检查确认已有仓库授权与具体能力；不要仅因某个 CLI 未登录就重复要求授权。工具能力不足与访问被拒绝分别报告；不得为绕过拒绝或安全限制而更换环境、账户或访问路径。

开工前一次盘点端到端所需的全部权限（仓库、分支、连接器、云端环境、评审服务等），集中请用户授权，不走一步补一步。之后发现还需要新的权限，按「超出已批准范围」停下请示。密钥与凭据不落 agent 工作机；需要凭据的步骤在用户本机或已授权的连接器中完成。

#### 职责分离
需求与架构评审、实现与调试、独立代码评审分开执行。独立评审方式由项目明确配置，不把实现者自查、代理自评或 CI 通过当作独立评审。

#### 默认分工（项目可覆盖）
- 设计与评审：Claude，默认模型 Opus 5.5、档位 High，在挂载了本仓库的私有 Claude Project 中进行。评审包上传完整文件（不只给 diff 或摘要），并明确说明不要依赖 Project 知识库里的旧版规格。
- 编码：需要用 Codex 时优先用 Codex 云端，默认模型 GPT-6.1 Sol、档位 Extra High。Cursor Agent 可用，但不是首选。
- 技术研究：技术方案、API 用法、官方文档、平台规则类问题，交技术研究角色查官方原文并附链接，再实现或拍板；不凭印象作答，也不把这类问题丢回给用户。项目没有配置技术研究角色时，由负责编程的 agent 自己查官方文档，并附上来源链接。
- 模型与档位核对：启动任何设计、评审或编码任务前，先确认能选到指定的模型与档位；选不到就停下回报，不得自行更换模型或降档。

#### 闭环
1. 写清问题、触发条件、目标与非目标；重大变更先写 [spec](specs/TEMPLATE.md)。
2. 先做规格/设计评审，确认验收、风险、回退与批准，再实现。
3. 先提交失败复现或验收测试，再最小修复。失败须为目标行为断言失败，不是安装或导入错误。若禁止提交/推送，只准备变更并记录证据。
4. 运行相关及项目要求的完整检查，逐条记录结果、SHA、环境和未覆盖范围。纯文档变更说明行为测试不适用原因。
5. PR 列验收清单与风险分级，请独立评审。评审必须对应当前 head；head 改变后重新检查受影响范围和评审有效性。
6. 合并前重新读取精确 head/base、PR 状态、所有适用 CI、独立评审及最终署名方案，并核对当前 head 与用户批准时写明的 SHA 一致。任一条件不符立即停止。
7. 仅按项目已批准策略合并。遇保护或并发拒绝不得绕过；不得改设置、强推或跳过检查求通过。
8. 合并后回读 main、PR 状态及实际提交署名；事后审计不能替代合并前检查。归档验收、剩余风险与回退方法。

#### 每批闭环与分级处理
本节细化上面闭环的第 3–5 步。每一批改动按同一循环执行：
1. 实现：实现者（默认 Codex 云端）按任务书实现。
2. 加强测试：适用时做竞态检测（例如 Go 的 `-race`）；多次连跑；功能开关/noop 的两种极性都覆盖。
3. 评审：评审者（默认 Claude）评审当前改动并给出决策；对每条 S2 标明是否挡合并或主流程，未标明的按“挡”处理。
4. 按分级处理。采用 MVP 迭代：每轮只修 S1 和挡合并或主流程的 S2，不为修完所有小问题而无限延长本轮：
   - S1（阻断）或测试不过：直接驳回，写重写任务书交实现者在本轮重做，不问用户。
   - 挡合并或挡主流程的 S2：实现者在本轮修完后重新跑同一套测试并复评，不问用户。
   - 其余 S2、S3 和小问题：登记到下一轮，按评审决策推进到下一个停止点，本轮不再继续修。

登记不等于豁免：登记项写明级别、来源和对应的 head，放在项目指定的位置（见适配表「下一轮登记位置」），请求合并批准时一并列出。测试不过和停止点事项不能登记顺延。

加强测试和独立评审合成一轮交接：写一份同时覆盖两者的任务书，该轮结束时只回报一次；只有中途卡住时才提前上报。停止点事项和扫描命中不受此限，出现即停下上报。

卡住时依次尝试：查官方文档、参考同类成熟项目、问评审者、换方法重试。穷尽这些手段后才上报，并附已尝试的方法与证据。

#### Spec 和证据
验收条目映射测试文件、用例名、输入与预期，区分本地/集成/生产。spec 不代替批准。CI、独立评审、合并和部署是不同门禁；本地通过不等于线上通过。失败、阻塞、跳过、未运行分别记录，不把缺失检查写成成功。

#### 重大变更与需用户批准的停止点
重大变更至少包括权限/安全边界、仓库保护/required checks、凭据、对外发布/部署、数据删除或不可逆迁移、依赖主版本升级及跨多个子系统的大重构。项目可细化阈值；是否属于重大变更拿不准时先确认。批准记录写范围及条件，不保存敏感数据。

除重大变更外，以下事项也必须停下等用户批准：
- 合并（批准只对批准时的 head 有效，见下文「批准与 head 绑定」）；
- 部署（每次部署单独批准，不能沿用前一次部署批准；批准只对指明的版本有效）；
- 改写已推送历史；
- 凭据；
- 真实隐私命中（扫描命中一律先停下上报，见「隐私与发布」；确认为真实命中后，如何处理须经用户批准）；
- 政策文件（`.github/**`、扫描脚本、`.gitleaks.toml`、`.githooks` 等）；
- 新增依赖；
- 超出已批准范围；
- 重大取舍；
- 放宽测试或门禁保证。

上述批准必须来自用户（项目负责人）本人。其他 agent 或 bot 给出的“批准”不算用户批准，包括评审 agent 的通过结论和平台上 bot 账号的 approve。不属于上述事项的，在已授权范围内按「每批闭环与分级处理」自动推进。

批准与 head 绑定：合并批准只对批准时的那个 head（提交 SHA）有效；请求批准和记录批准时都写明该 SHA。批准之后，只要有新提交、rebase 或 base 变化使 head 改变，原批准即失效，必须重新验证（重跑受影响的检查，确认评审仍对应当前 head），并重新取得用户本人的批准，才能合并。部署批准同理，只对批准时指明的具体版本（提交 SHA 或发布 tag）有效；要部署的版本变了，须重新取得批准。

新增依赖（如 `go.mod`/`go.sum`、Cargo、npm 等）先做架构评估（必要性、替代方案、维护与许可、安全影响），评估通过并获批后再落地。

每次合并都要评审在先，不能先合后审。CI 通过或代理自评不能替代规定的评审。Draft PR 由人决定何时转为 ready 及是否合并。

#### 交接与任务书
- 任务书写明：基点 commit、分支名、Draft PR 目标分支、提交顺序（先改规格 → 先红的测试 → 实现）、硬约束、验收标准和署名 trailer。
- 署名 trailer 照抄仓库署名脚本或策略文件原文（见适配表「署名 trailer 来源」），不凭记忆书写；项目没有署名脚本或策略文件时，按「署名」中的来源 trailer 规则填写（云端运行只写 `Code-Tool`，本地电脑运行写 `Code-Tool` 并加 `Host-Id`）。
- 给编码工具的输入框只粘贴任务书正文，不夹带对话或说明；发送前截图核对。
- 项目指定的云端编码环境（默认见「默认分工」）是完整工作环境：可以读写、向约定分支推送（分支前缀见适配表，默认 `codex/*`）、开 Draft PR。不要给它下“只读”或“不推送”的指令，除非用户在该任务中明确收窄权限。取回改动时，优先让它推送约定分支并开 Draft PR。这只覆盖约定分支和 Draft PR，不改变停止点：合并、部署、改写已推送历史、政策文件等仍按「重大变更与需用户批准的停止点」停下等批准。
- 提交云端任务前核对：环境是云端而不是本地，模型与档位正确，且该环境的 Git 托管连接已授权本仓库（实际查看连接状态，确认能看到本仓库和目标分支）。任务入口没有分支选择器时（例如网页任务），把 base 分支写在任务正文里。提交后必须有人跟到任务完成，并主动汇报结果。
- 遇到授权拒绝或分支不可用时，先重试并实际验证当前的授权与分支状态，不沿用旧的失败结论，也不因此搁置任务。验证后仍被拒绝的，按「环境与已有授权」如实上报，不绕过。

#### 署名
先用只读检查核实目标仓库已有授权和当前工具能力；命令行未登录不等于连接器无权访问，不应因此要求重复授权。Git author/committer 元数据须如实保留和披露，不伪装身份；工具支持且项目允许时使用可识别的 AI 作者名称。若工具不能指定 AI author，且项目规则允许，可保留工具实际设置的连接账户身份，在提交信息明确写明 AI authored 并加 `Agent: <实际来源标识>`，在 PR 披露 AI 来源、实际 author/committer 及工具限制，不宣称作者字段已经设为 AI。若项目强制 AI 作者名称、DCO 或特定签名且当前路径无法满足，停止相关提交/发布并请示，不绕过、不擅自改规则或创建凭据。trailer 的具体键名与格式以项目仓库的署名脚本或策略文件原文为准。

项目没有署名脚本或策略文件时，在其他 trailer（如 `Agent: <实际来源标识>`）之外，按编码 agent 的运行环境加来源 trailer：
- 在云端环境运行时（例如 Codex 云端）：只写工具本身的标识，例如 `Code-Tool: codex <模型> <档位>`，不写 Host-Id。
- 在本地电脑上运行时：同样写 `Code-Tool: codex <模型> <档位>`，并加 `Host-Id: <host-id>`；Host-Id 只写为该电脑约定的 id，不写真实主机名。两行都写，不能只写 Host-Id。

文档、示例和任务书模板只用占位符，不写任何真实主机名。注意：在本地电脑上运行时采用这条，host id 会出现在公开的提交记录里。

人工提交使用本人选择姓名、邮箱及 `Agent: human`；人工批准不改变 AI 实作来源。不以邮箱域名判定 AI，不强制人工 noreply，不擅自修改全局身份或账号隐私设置，不在文档/PR 正文复制真实邮箱。

服务端 squash/rebase/merge 可能重写元数据，必须在合并前核实。若项目批准采用保留原提交的 fast-forward：确认 base 是 head 祖先、全部待合入提交通过署名检查、远端值仍未变化，只用普通非强推更新精确已核验 head；保护拒绝即停止。不把此方案设为所有项目通用默认。

#### 隐私与发布
个人协调信息与项目开发规则分离。公开发布只按经审核的白名单导出，并扫描内容和要发布的历史；不得复制私人仓历史。示例使用占位符和文档保留域名，不写凭据、私人地址或原始线上日志。隐私扫描不能替代人工审查。

推送和发布前做隐私与凭据安全检查：
- 扫描和推送必须分两条命令执行，不得放在同一条命令里。
- 检查范围至少包括个人信息、主机名、内网域名、邮箱（noreply 和文档保留域名除外）、IP、token 和密钥，并覆盖待推送的改动、提交说明和 PR 正文。
- 扫描有任何命中（包括认为是误报的），都先停下上报，不得自行判断后推送，也不得自行把命中加入白名单、忽略规则或基线。

提交信息、PR 正文和其他对外内容不写人名、花费或用量、地址或城市；Git author/committer 元数据按「署名」如实保留。

#### 分支、PR 与文档交付
- 没有用户指示时不删除分支。已关闭 PR 的分支不删除。「清理无效 PR」指关闭功能重复的 PR，不指删除分支。
- 登记到下一轮的事项写在项目指定的位置（见适配表「下一轮登记位置」）；未指定时写在相关 PR 的评论和交接记录中，不为登记去改仓库设置。
- 项目文档以仓库为准，改动走分支和 PR；交付时给仓库链接，不发压缩包。

#### 复盘与自动学习
- 运行不顺或被用户指出问题时，主动复盘：把教训写进 agent 记忆或技能，并通过分支和 PR 更新本 workflow 的变更记录。复盘做法以官方文档和成熟开源项目为参照。
- 每项任务完成后汇报结果，并说明是否还有下一步。加强测试和独立评审作为一轮交接时，按「每批闭环与分级处理」只回报一次。

#### 术语、方案与沟通
- 专有名词按该项目已有文档和约定的含义理解；含义不清、又会影响方向时，先确认再开工。
- 不要擅自改用户（项目负责人）已定下的部署方案或实测方案；有疑问先确认。
- 需要用户本人登录、授权或确认时，立刻列出具体步骤请其操作，不搁置等待，也不索要密码、token 或私钥。

#### 项目适配表
- 测试与 lint：<COMMANDS>
- 隐私/secret/SAST：<COMMANDS_OR_UNCONFIGURED>
- 独立评审机制：<REVIEWER_OR_SERVICE>（默认 Claude，Opus 5.5 High，私有 Claude Project）
- 编码工具与档位：<CODING_TOOL_AND_TIER>（默认 Codex 云端，GPT-6.1 Sol，Extra High；云端任务的约定分支前缀默认 `codex/*`）
- 技术研究角色：<RESEARCH_ROLE>（项目没有配置时：由负责编程的 agent 自己查官方文档并附来源链接）
- 署名 trailer 来源：<ATTRIBUTION_SCRIPT_OR_POLICY>（项目没有署名脚本或策略文件时：云端运行写 `Code-Tool: <工具> <模型> <档位>`，本地电脑运行写 `Code-Tool: <工具> <模型> <档位>` 并加 `Host-Id: <host-id>`，见「署名」；本地运行时 host id 会出现在公开的提交记录里）
- 下一轮登记位置：<BACKLOG_LOCATION>（项目没有指定时：相关 PR 的评论和交接记录）
- required checks：<CHECK_NAMES>
- 合并策略及批准依据：<STRATEGY_AND_APPROVAL>
- 发布与部署：<SEPARATE_AUTHORIZATION_AND_RUNBOOK>

#### 证据与生成文件
观察结果、推导与未知分开记录。模拟测试不代表真实环境验证；标明实际测试条件和样本范围。生成文件应从项目指定源文件再生成，不直接手改生成结果；项目没有此类文件则不适用。

#### 变更记录
- 2026-11-15 — v2.2（来源与评审依据见模板仓库的 `docs/reviews/v2.2-review.md`）
  - 新增：
    - 「每批闭环与分级处理」：MVP 迭代，每轮只修 S1 和挡合并或主流程的 S2，其余 S2、S3 和小问题登记到下一轮；评审者标明每条 S2 是否挡合并或主流程；登记不等于豁免，请求合并批准时一并列出；加强测试和独立评审合成一轮交接，只回报一次，停止点事项和扫描命中仍然出现即报。
    - 「交接与任务书」：项目指定的云端编码环境是完整工作环境，可读写、向约定分支推送、开 Draft PR，不下“只读”或“不推送”指令，且不改变停止点；提交前确认 Git 托管连接已授权本仓库；任务入口没有分支选择器时把 base 分支写在任务正文里；遇到授权拒绝或分支不可用先重试并实际验证，不沿用旧的失败结论，也不搁置。
    - 「术语、方案与沟通」一节：专有名词按项目含义理解，含义不清且会影响方向时先确认；不擅自改已定的部署或实测方案；需要用户本人登录或授权时立刻列出步骤。
    - 「项目适配表」：「下一轮登记位置」一行；「编码工具与档位」一行补充云端任务的约定分支前缀。
  - 调整：
    - 「重大变更与需用户批准的停止点」：写明批准必须来自用户本人，其他 agent 或 bot 的“批准”不算；补充“每次部署单独批准”，并新增“放宽测试或门禁保证”（收紧，不放宽原清单）。
    - 「重大变更与需用户批准的停止点」和「闭环」第 6 步：批准与 head 绑定。合并批准只对批准时的 head（提交 SHA）有效；新提交、rebase 或 base 变化使 head 改变后，须重新验证并重新取得用户本人批准；部署批准同理，只对指明的具体版本有效（收紧）。
    - 「署名」：Host-Id 只写约定的 id，不写真实主机名。
    - 「隐私与发布」：列出扫描的最低检查范围；任何命中都不得自行加入白名单；对外内容除不写花费或用量外，也不写人名、地址或城市。
    - 「分支、PR 与文档交付」：没有用户指示时不删除分支；登记项的默认位置。
- 2026-11-12 — v2.1
  - 新增：
    - 「默认分工（项目可覆盖）」：设计与评审、编码、技术研究的默认角色与模型，以及启动前核对模型与档位。
    - 「默认分工」：项目没有配置技术研究角色时，由负责编程的 agent 自己查官方文档并附来源链接（负责人决定）。
    - 「每批闭环与分级处理」「交接与任务书」「分支、PR 与文档交付」「复盘与自动学习」四节。
    - 「环境与已有授权」：开工前集中授权；密钥与凭据不落 agent 工作机。
    - 「署名」：项目没有署名脚本或策略文件时，按编码 agent 的运行环境加来源 trailer：云端运行只写工具标识（如 `Code-Tool: codex <模型> <档位>`），不写 Host-Id；本地电脑运行写同样的工具标识并加 `Host-Id: <host-id>`。文档只用占位符，并提示本地运行时 host id 会出现在公开的提交记录里（负责人决定）。
    - 「隐私与发布」：扫描与推送分两条命令；任何命中先停下上报；提交与 PR 不写花费或用量。
    - 「项目适配表」：编码工具与档位、技术研究角色、署名 trailer 来源三行，后两行写明项目没有配置时的默认做法。
  - 调整：
    - 原「重大变更」一节改为「重大变更与需用户批准的停止点」，新增停止点清单、新增依赖的架构评估、每次合并评审在先，以及 Draft PR 由人决定转 ready 和合并。
    - 原文「项目可细化阈值；不确定时先确认」改为仅针对「是否属于重大变更」的判断先确认；技术类疑问改为交技术研究角色查官方原文，卡住时先自行穷尽手段再上报，不再一律先问用户。
    - 「职责分离」：不能当作独立评审的情形新增「代理自评」。
    - 「署名」：trailer 键名与格式以仓库署名脚本或策略文件原文为准。
- 2026-11-09 — v2：初始通用基线。

## English

### Collaboration workflow v2.2

> Generic baseline. Fill in roles, commands, checks, and merge strategy when adopting it for a project. This document does not authorize external actions; "proceed automatically" applies only within the authorized scope. Tools and models named here are defaults; projects may override them in the configuration table. "The user" means the person who authorized the task (usually the project owner). See "Change log" at the end for version changes.

#### Environment and existing access
Configurable environment default: prefer the cloud computer. Follow the user’s task-specific environment choice; when work depends on local files, apps or signed-in sessions, select a suitable environment with authorized access to those resources. Verify the current environment and target before acting. Prefer an existing connected API/connector that supports the operation, and use read-only checks to establish existing repository access and concrete capabilities; do not request duplicate authorization merely because a CLI is not logged in. Distinguish unsupported capabilities from denied access; never switch environments, accounts or access paths to evade a denial or security restriction.

Before starting, inventory every permission the end-to-end task needs (repositories, branches, connectors, cloud environments, review services, and so on) and request them from the user in one batch, rather than asking step by step. If a new permission turns out to be needed later, stop and ask under "work beyond the approved scope". Keys and credentials never land on the agent's work machine; steps that need credentials run on the user's own computer or through an authorized connector.

#### Separation of responsibilities
Perform requirements and architecture review, implementation and debugging, and independent code review separately. The project must explicitly configure its independent review mechanism. An implementer's self-review, an agent's self-assessment, or passing CI does not constitute independent review.

#### Default roles (projects may override)
- Design and review: Claude, default model Opus 5.5, tier High, in a private Claude Project with this repository attached. Upload complete files in the review package (not only a diff or summary), and state explicitly not to rely on older specs in the Project knowledge base.
- Coding: when Codex is needed, prefer Codex cloud, default model GPT-6.1 Sol, tier Extra High. Cursor Agent may be used but is not the first choice.
- Technical research: questions about technical approaches, API usage, official documentation, or platform rules go to the technical research role, which checks the official source and attaches links before anything is implemented or decided. Do not answer from memory, and do not hand such questions back to the user. If the project has not configured a technical research role, the agent responsible for coding checks the official documentation itself and attaches source links.
- Model and tier check: before starting any design, review, or coding task, confirm that the specified model and tier can be selected; if not, stop and report. Do not switch models or lower the tier on your own.

#### End-to-end workflow
1. Define the problem, triggering conditions, goals, and non-goals. Write a [spec](specs/TEMPLATE.md) first for major changes.
2. Review the specification/design first, confirming acceptance criteria, risks, rollback, and approvals before implementing.
3. Commit the failing reproduction or acceptance tests first, then make the smallest fix. The failure must be an assertion about the target behavior, not an installation or import error. If commits/pushes are prohibited, prepare changes and record evidence only.
4. Run relevant checks and all full checks required by the project. Record each result, SHA, environment, and uncovered scope. For documentation-only changes, explain why behavioral tests are not applicable.
5. Include an acceptance checklist and risk level in the PR and request independent review. Review must correspond to the current head; after head changes, recheck affected areas and review validity.
6. Before merging, reread the exact head/base, PR status, all applicable CI, independent review, and final attribution plan, and confirm that the current head is the SHA stated when the user approved. Stop immediately if any condition is unmet.
7. Merge only using the project's approved strategy. Do not bypass protection or concurrent-update rejections; do not change settings, force-push, or skip checks to get through.
8. After merging, reread main, PR status, and actual commit attribution. Post-merge auditing does not replace pre-merge checks. Archive acceptance results, remaining risks, and the rollback method.

#### Per-batch loop and severity handling
This section details steps 3–5 of the workflow above. Every batch of changes follows the same loop:
1. Implement: the implementer (default Codex cloud) implements according to the task brief.
2. Strengthened tests: race detection where applicable (such as Go's `-race`); repeated runs; and coverage of both polarities of feature switches/noop paths.
3. Review: the reviewer (default Claude) reviews the current changes and decides, marking for each S2 issue whether it blocks a merge or the main flow; an unmarked S2 issue is treated as blocking.
4. Handle by severity. Use MVP iteration: each round fixes only S1 issues and S2 issues that block a merge or the main flow, and is not extended indefinitely to fix every small item:
   - S1 (blocking) or failing tests: reject directly and write a rewrite brief for the implementer to rework in this round, without asking the user.
   - S2 that blocks a merge or the main flow: the implementer fixes it in this round, then the same tests are rerun and the change is re-reviewed, without asking the user.
   - Remaining S2 issues, S3 issues, and small items: log them for the next round and proceed to the next stop point per the review decision; do not keep fixing them in this round.

Logging an item does not waive it: record its severity, source, and the head it applies to in the location the project designates (see "Next-round log location" in the configuration table), and list the logged items when requesting merge approval. Failing tests and stop-point items cannot be deferred by logging them.

Strengthened tests and independent review form one handoff: write a single brief that covers both, and report once when that round finishes; report earlier only if work gets stuck mid-round. Stop-point items and scan hits are not subject to this limit: stop and report as soon as they occur.

When stuck, try in order: check official documentation, look at comparable mature projects, ask the reviewer, and retry with a different approach. Escalate only after these are exhausted, with the attempts and evidence attached.

#### Specs and evidence
Map acceptance items to test files, case names, inputs, and expected results, distinguishing local/integration/production validation. A spec is not approval. CI, independent review, merging, and deployment are separate gates; passing locally does not mean passing in production. Record failures, blockers, skipped steps, and steps not run separately. Do not report missing checks as successful.

#### Major changes and stop points that require user approval
Major changes include, at minimum, permission/security boundaries, repository protections/required checks, credentials, external releases/deployments, data deletion or irreversible migrations, major dependency upgrades, and large refactors across multiple subsystems. Projects may refine thresholds; when unsure whether something is a major change, ask first. Approval records must state scope and conditions without storing sensitive data.

In addition to major changes, the following must also stop and wait for user approval:
- merging (the approval is valid only for the head it was given for; see "Approvals are bound to a head" below);
- deployment (each deployment needs its own approval; do not reuse a previous deployment approval; the approval is valid only for the version it names);
- rewriting pushed history;
- credentials;
- real privacy hits (any scan hit means stop and report first, see "Privacy and release"; once a hit is confirmed as real, how to handle it needs user approval);
- policy files (`.github/**`, scan scripts, `.gitleaks.toml`, `.githooks`, and similar);
- new dependencies;
- work beyond the approved scope;
- major trade-offs;
- relaxing a test or gate guarantee.

These approvals must come from the user (the project owner) in person. An "approval" from another agent or bot is not user approval; this includes a review agent's passing verdict and an approve from a bot account on the platform. Anything not listed above proceeds automatically within the authorized scope under "Per-batch loop and severity handling".

Approvals are bound to a head: a merge approval is valid only for the head (commit SHA) it was given for; state that SHA both when requesting the approval and when recording it. After approval, if a new commit, a rebase, or a base change alters the head, the approval lapses: revalidate (rerun the affected checks and confirm that the review still corresponds to the current head) and obtain the user's approval in person again before merging. A deployment approval is bound in the same way to the specific version (commit SHA or release tag) named when it was given; if the version to be deployed changes, obtain approval again.

New dependencies (for example `go.mod`/`go.sum`, Cargo, npm) need an architecture assessment first (necessity, alternatives, maintenance and licensing, security impact); land them only after the assessment passes and approval is given.

Every merge needs review first; never merge first and review later. Passing CI or an agent's self-assessment cannot replace the required review. A person decides when a Draft PR becomes ready and whether it is merged.

#### Handoff and task briefs
- A task brief states the base commit, branch name, Draft PR target branch, commit order (spec changes → failing tests first → implementation), hard constraints, acceptance criteria, and the attribution trailer.
- Copy the attribution trailer verbatim from the repository's attribution script or policy file (see "Attribution trailer source" in the configuration table); do not write it from memory. If the project has no attribution script or policy file, follow the source trailer rule under "Attribution" (only `Code-Tool` when running in the cloud; `Code-Tool` plus `Host-Id` when running on a local computer).
- Paste only the brief body into the coding tool's input box, with no conversation or commentary; check it with a screenshot before sending.
- The cloud coding environment the project designates (default under "Default roles") is a full working environment: it may read and write, push to the agreed branches (branch prefix in the configuration table, default `codex/*`), and open a Draft PR. Do not instruct it to be "read-only" or "not to push," unless the user explicitly narrows permissions for that task. To retrieve changes, prefer having it push the agreed branch and open a Draft PR. This covers only the agreed branches and Draft PRs and does not change the stop points: merging, deployment, rewriting pushed history, policy files, and the rest still stop and wait for approval under "Major changes and stop points that require user approval".
- Before submitting a cloud task, verify that the environment is cloud, not local, that the model and tier are correct, and that the environment's Git hosting connection is authorized for this repository (check the connection state itself and confirm that this repository and the target branch are visible). If the task entry point has no branch picker (for example a web task), state the base branch in the task body. After submission, someone must follow it through to completion and report the result proactively.
- On an authorization denial or an unavailable branch, retry and actually verify the current authorization and branch state; do not reuse an earlier failure conclusion, and do not shelve the task on that basis. If access is still denied after verification, report it truthfully under "Environment and existing access"; do not work around it.

#### Attribution
First use read-only checks to verify existing access to the target repository and the capabilities of the current tools. A CLI without a login does not mean the connector lacks access; do not request duplicate authorization on that basis. Preserve and disclose Git author/committer metadata truthfully, without impersonation; use an identifiable AI author name when the tool supports it and project rules allow it. If the tool cannot set an AI author and project rules permit the fallback, retain the connected-account identity actually set by the tool, explicitly state AI authored in the commit message, add `Agent: <actual-source-identifier>`, and disclose AI origin, the actual author/committer and the tool limitation in the PR. Do not claim that the author field is an AI identity. If mandatory AI-author, DCO or signing rules cannot be met through the current path, stop the affected commit/publication and ask for direction; do not bypass requirements, change rules or create credentials on your own. Take the exact trailer keys and format from the project's attribution script or policy file verbatim.

If the project has no attribution script or policy file, add source trailers alongside the other trailers (such as `Agent: <actual-source-identifier>`), depending on where the coding agent runs:
- In a cloud environment (for example Codex cloud): record only the tool's own identifier, such as `Code-Tool: codex <model> <tier>`; do not add Host-Id.
- On a local computer: record the same `Code-Tool: codex <model> <tier>` and add `Host-Id: <host-id>`; Host-Id carries only the id agreed for that computer, never its real host name. Write both lines, never Host-Id alone.

Documents, examples, and task-brief templates use only placeholders, never a real host name. Note: when this rule is applied on a local computer, the host id appears in the public commit history.

Human commits use the person’s chosen name, email and `Agent: human`; human approval does not change AI implementation origin. Do not infer AI origin from email domains, require humans to use noreply, change global identity or account privacy settings without authorization, or copy real email addresses into document/PR body text.

Server-side squash/rebase/merge may rewrite metadata; verify this before merging. If the project approves fast-forward merging to preserve original commits: confirm that base is an ancestor of head, all incoming commits pass attribution checks, and remote values remain unchanged. Use only a normal, non-force push to update to the exact verified head; stop if protections reject it. Do not make this the universal default for all projects.

#### Privacy and release
Keep personal coordination information separate from project development rules. For public releases, export only a reviewed allowlist and scan both contents and the history to be published; do not copy private repository history. Use placeholders and documentation-reserved domains in examples, with no credentials, private addresses, or raw production logs. Privacy scanning does not replace human review.

Run privacy and credential checks before every push and release:
- Scanning and pushing must be two separate commands, never one.
- The check covers at least personal information, host names, internal domains, email addresses (except noreply and documentation-reserved domains), IP addresses, tokens, and keys, across the changes to be pushed, the commit messages, and the PR body.
- If the scan reports any hit (including one you believe is a false positive), stop and report it first; do not decide on your own and push, and do not add the hit to an allowlist, ignore rule, or baseline on your own.

Commit messages, PR bodies, and other outward-facing content do not include personal names, cost or usage figures, or addresses or cities; Git author/committer metadata is preserved truthfully under "Attribution".

#### Branches, PRs, and document delivery
- Do not delete branches without a user instruction. Do not delete the branches of closed PRs. "Clean up invalid PRs" means closing PRs that duplicate functionality, not deleting branches.
- Log next-round items in the location the project designates (see "Next-round log location" in the configuration table); if none is designated, log them in comments on the relevant PR and in the handoff record. Do not change repository settings in order to log them.
- The repository is the source of truth for project documents; changes go through branches and PRs. Deliver a repository link, not an archive file.

#### Retrospectives and continuous learning
- When a run goes badly or the user points out a problem, run a retrospective proactively: record the lesson in agent memory or a skill, and update this workflow's change log through a branch and PR. Base the retrospective practice on official documentation and mature open-source projects.
- After each task, report the result and state whether there is a next step. When strengthened tests and independent review form one handoff, report once as described under "Per-batch loop and severity handling".

#### Terms, plans, and communication
- Interpret project-specific terms according to the project's own documents and conventions; if the meaning is unclear and it would affect direction, confirm before starting work.
- Do not unilaterally change a deployment plan or a real-environment test plan the user (the project owner) has already set; confirm first when in doubt.
- When a step needs the user to sign in, authorize, or confirm in person, list the concrete steps for them immediately; do not shelve the task while waiting, and do not ask for passwords, tokens, or private keys.

#### Project configuration
- Tests and lint: <COMMANDS>
- Privacy/secrets/SAST: <COMMANDS_OR_UNCONFIGURED>
- Independent review mechanism: <REVIEWER_OR_SERVICE> (default Claude, Opus 5.5 High, private Claude Project)
- Coding tool and tier: <CODING_TOOL_AND_TIER> (default Codex cloud, GPT-6.1 Sol, Extra High; the agreed branch prefix for cloud tasks defaults to `codex/*`)
- Technical research role: <RESEARCH_ROLE> (if the project has not configured one: the agent responsible for coding checks the official documentation itself and attaches source links)
- Attribution trailer source: <ATTRIBUTION_SCRIPT_OR_POLICY> (if the project has no attribution script or policy file: `Code-Tool: <tool> <model> <tier>` when running in the cloud, `Code-Tool: <tool> <model> <tier>` plus `Host-Id: <host-id>` when running on a local computer, see "Attribution"; for local runs the host id appears in the public commit history)
- Next-round log location: <BACKLOG_LOCATION> (if the project has not designated one: comments on the relevant PR and the handoff record)
- required checks: <CHECK_NAMES>
- Merge strategy and approval basis: <STRATEGY_AND_APPROVAL>
- Release and deployment: <SEPARATE_AUTHORIZATION_AND_RUNBOOK>

#### Evidence and generated files
Record observations, deductions, and unknowns separately. Simulated tests do not establish validation in a real environment; state the actual test conditions and sample scope. Regenerate generated files from project-designated source files instead of hand-editing the output. This does not apply if the project has no such files.

#### Change log
- 2026-11-15 — v2.2 (sources and review basis: `docs/reviews/v2.2-review.en.md` in the template repository)
  - Added:
    - "Per-batch loop and severity handling": MVP iteration, where each round fixes only S1 issues and S2 issues that block a merge or the main flow, and the remaining S2 issues, S3 issues, and small items are logged for the next round; the reviewer marks whether each S2 issue blocks a merge or the main flow; logging does not waive an item, and logged items are listed when requesting merge approval; strengthened tests and independent review form one handoff with one report, while stop-point items and scan hits are still reported as soon as they occur.
    - "Handoff and task briefs": the cloud coding environment the project designates is a full working environment that may read and write, push to the agreed branches, and open a Draft PR; it is not given "read-only" or "do not push" instructions, and the stop points are unchanged. Confirm that the Git hosting connection is authorized for this repository before submitting; state the base branch in the task body when the task entry point has no branch picker; on an authorization denial or an unavailable branch, retry and actually verify rather than reusing an earlier failure conclusion or shelving the task.
    - The "Terms, plans, and communication" section: interpret project-specific terms as the project defines them and confirm first when the meaning is unclear and would affect direction; do not unilaterally change a deployment or real-environment test plan that has been set; list the steps immediately when the user must sign in or authorize in person.
    - "Project configuration": the "Next-round log location" row; the "Coding tool and tier" row now includes the agreed branch prefix for cloud tasks.
  - Changed:
    - "Major changes and stop points that require user approval": approvals must come from the user in person, and an "approval" from another agent or bot does not count; clarifies that each deployment needs its own approval, and adds "relaxing a test or gate guarantee" (a tightening, not a loosening of the previous list).
    - "Major changes and stop points that require user approval" and step 6 of "End-to-end workflow": approvals are bound to a head. A merge approval is valid only for the head (commit SHA) it was given for; after a new commit, a rebase, or a base change alters the head, revalidate and obtain the user's approval in person again; a deployment approval is likewise valid only for the specific version it names (a tightening).
    - "Attribution": Host-Id carries only the agreed id, never a real host name.
    - "Privacy and release": the minimum scope of the scan is listed; no hit may be added to an allowlist on the agent's own decision; outward-facing content also omits personal names, addresses, and cities, in addition to cost or usage figures.
    - "Branches, PRs, and document delivery": do not delete branches without a user instruction; default location for logged items.
- 2026-11-12 — v2.1
  - Added:
    - "Default roles (projects may override)": default roles and models for design and review, coding, and technical research, plus the pre-start model and tier check.
    - "Default roles": if the project has not configured a technical research role, the agent responsible for coding checks the official documentation itself and attaches source links (owner's decision).
    - Four sections: "Per-batch loop and severity handling", "Handoff and task briefs", "Branches, PRs, and document delivery", and "Retrospectives and continuous learning".
    - "Environment and existing access": one-batch authorization before starting; keys and credentials never land on the agent's work machine.
    - "Attribution": if the project has no attribution script or policy file, add source trailers depending on where the coding agent runs: in the cloud, only the tool identifier (such as `Code-Tool: codex <model> <tier>`), with no Host-Id; on a local computer, the same tool identifier plus `Host-Id: <host-id>`. Documents use only placeholders, with a note that for local runs the host id appears in the public commit history (owner's decision).
    - "Privacy and release": separate scan and push commands; stop and report on any hit; no cost or usage figures in commits and PRs.
    - "Project configuration": three rows for coding tool and tier, technical research role, and attribution trailer source; the last two state the default when the project has not configured them.
  - Changed:
    - The former "Major changes" section is now "Major changes and stop points that require user approval", adding the list of stop points, the architecture assessment for new dependencies, review before every merge, and a person deciding when a Draft PR becomes ready and whether it is merged.
    - The original "Projects may refine thresholds; ask first when uncertain" now applies only to deciding whether something is a major change; technical questions go to the technical research role for the official source, and when stuck the agent exhausts its own options before escalating, instead of always asking the user first.
    - "Separation of responsibilities": an agent's self-assessment is added to what does not constitute independent review.
    - "Attribution": trailer keys and format come from the repository's attribution script or policy file verbatim.
- 2026-11-09 — v2: initial generic baseline.
