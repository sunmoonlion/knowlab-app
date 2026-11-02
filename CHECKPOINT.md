# 2026-11-02 第二轨 Wind 通用查询与年报证据候选

基线 `cea4e7c562400a5352b8ee9a445ab6fd86b2648e`，子仓先推 GitHub，再钉本仓。

- knowlab-backend: `7a54c10790794461ae82afcbb0b0df9176d7d73d`

可选 source_query 契约/消费锁同步，既有固定专家和巨潮原件路径保留。财务、公告年报等能力按官方七服务域发现；供应商文本/结构化回执不冒充 PDF 或页码。测试、初始失败和剩余门见子仓 CHECKPOINT 与 sunmoonun 的 Wind-W0评估续记。当前未生产启用，发布仍由 qwen 走 Jenkins。

#52 已发布修复在 knowbase/knowbench 子仓成为祖先；源码与已验证候选相同。实际第二轨演示还待正式服务配置、真实作者/方法与数据读工具链验证，不把取数探针当成业务通过。

以下是之前历史：

# 2026-11-02 第二轨：WP-10 作者/运行契约

## 最新增量：作者冻结、精确选版和动态测试页面

后端 `c5bf88606bf058ecf9dc586223188e984949160c`，前端
`a54e26c18c2ecaafe8385bc50d5052c8b69c516f`，均已推 GitHub
`feat/method-execution-view`。父仓同步 author-method/v1 说明与两个子仓钉版。
下文早期“未接页面/引用”的状态是历史记录，当前以后端和前端 CHECKPOINT 为准。

真实方法列表、作者冻结、选择确切版本、输入预览与引用保存已接既有 API；工作稿保留
规划和选择条件，绑定方法正文只读。作者测试展示全部最终成果及失效历史，兼容固定
专家。列表元信息来自本人最新方法版本，精确读版与摘要核验不变，无新增迁移。

后端全量 792 passed / 5 skipped，ruff/pyright 通过；前端 182 passed / 2 skipped，
typecheck/lint/i18n（931 keys）/build 通过。合成 fixture 在前后端交叉核验。
日志 `/tmp/luna-author-ui-lab-full.txt`、`/tmp/luna-author-flow-web-full.txt`。
agy 初稿中的硬编码目录/占位引用未纳入，未确认的财务方法仍仅为候选测试材料。

未发布，v3 外部准入仍关闭；真实作者流程与沙箱联合验收尚待进行。qwen 负责既有
发布候选，本轮没有操作 Jenkins、sources、线上数据或 Wind。

feat/dynamic-methods 已完成第一单元作者/运行契约，后端 dad9b0e20a8270747cfd999bacd7dafc3b2df6b0。显式 planning 与 selection 编译为 v3 动态目录；旧 v2 编译与摘要不变。作者确认方法单元、验收与工具边界，运行模型后续只能在冻结范围选择排序，不临时发明方法。计划存储/执行/UI 尚未实现，运行入口仍拒绝动态 v3。

父仓增加 knowmart v3 和 knowbench dynamic-methods/v1 消费锁，同步孵化 v1 请求/结果 schema（可选字段、planning/selection 建议路径）。三方样例来自真实编译器输出，无真实财务数据。完整本地回归 736 passed / 5 skipped，/tmp/luna-wp10-lab-full-final.txt；改动文件 lint 和域类型检查通过。全仓原有 1 个类型、11 个 lint 问题与解包基线逐条相同，无新增；见后端 CHECKPOINT.md。

下面是此前前置门的证据，不替代本次回归：

后端 70555a6：jsonschema 4.26.0 进入 dev 及锁，已有孵化/测试/清单 schema 校验不再 importorskip。补齐旧测试请求缺失的必填 runner，同时增加缺 runner 被拒的验证。此前省略 runner 的示例因缺依赖一直跳过，此次真实执行发现失败后修复，没有放宽 provider schema。

验证：`uv run --locked pytest -q tests/test_knowbot_workdraft_compile.py tests/test_knowbot_contract_locks.py tests/test_knowbot_incubation_contract.py tests/test_knowbot_test_run_contract.py`，71 passed；相关文件 ruff 通过；锁中已有运行依赖版本不变。输出 /tmp/luna-dynamic-contract-gates-lab-final.txt；初次失败保留于 /tmp/luna-dynamic-contract-gates-lab.txt。不是全仓/生产镜像/业务验收。

不改 qwen 正在发布的 fix/supply-delivery 候选；本分支未发布，不更新部署源，不调用 Wind。


## 2026-11-02 WP-10 单元 1a 第一段：作者方法主档

后端钉 422116eccd84a1d1ade34b1711378fc3c6a01d19，已推 GitHub feat/dynamic-methods。父仓新增 contracts/author-method/v1/definition.schema.json（后端 MethodDefinition 生成）与 README；不改既有运行清单或消费者锁，因为尚未接引用交付。

作者方法身份/版本独立保存，冻结需既有工作稿作者出处与编译核验；结构输入槽不再是来源专家别名。正常 web 身份/CSRF 下可冻结、精确读版、列自己的方法、做引用预览；未接页面。版本数据库不可变、同确认幂等、异载荷冲突、升版连续，旧版不被新工作稿覆盖。预览只验证目标工作稿的输入/上限/图，不保存引用或发布专家，不得用复制 step 代替后续引用工程。

迁移头 20261102_0025，前版 20261101_0024；空库退/升测试通过，非空方法历史阻止破坏性 downgrade，回退应用保留 schema。全量 760 passed / 5 skipped，新增故障案例后最终相关四文件 32 passed；整个 knowlab 后端 ruff/pyright 已通过，旧静态债务已清（CategoryCode 枚举及 schema 与原版完全一致）。详细证据见后端 CHECKPOINT.md。

下一段保存专家精确引用、选择/升级确认、编译版本与快照核验，并同步 knowmart/knowbench 契约/锁。当前只完成主档与预览，不能声称跨专家运行验收完成。发版仍由 qwen 使用现有 Jenkins，本段无发布、无线上改动、无 Wind 调用。


## 2026-11-02 WP-10：作者编译覆盖检查

钉后端 256e726、前端 6699ce0，子仓先推 GitHub。covers 源/目标类型在作者工作稿
编译时检查，错误不能冻结；历史快照仍按原 v1 翻译核摘要，新保存/绑定必须过当前
检查。前端只对齐真实合成夹具，无产品 UI 变化。后端全量 807 passed / 5 skipped，
前端 182 passed / 2 skipped；两端静态检查通过，详见子仓 CHECKPOINT。
开发分支未发布，不改契约版本/迁移/生产采单，不把 agy 财务夹具作为生产资产。


## 2026-11-02：kimi 方法库修复集成

后端 27286e5、前端 72db347 已推送 GitHub fix/kimi-method-validation。
合入 Luna a0b71b8，保留 Wind consumer 契约，不回退既有开发。
同步 knowbench 通用数值/行验收扩展 v1/v2 consumer 锁；54 项合成运行正反例，
作者侧回归 110 passed/1 skipped，隔离 DB 15 passed，前端 183 passed/2 skipped。
本批只是工程修复与候选测试资产；不宣称生产方法库导入或实战验收完成。


追加复核：后端 f1bbcd3 修复 M20 来源标签为中立输入槽，跨专家别名装配测试通过。
合回 Luna 后完整相关编译/方法/跨应用锁回归 117 passed（无跳过），运行侧 234 passed；
最终 GitHub 子仓均可获取。金融真实样本与业务内容待验，不导入生产候选夹具。

## 2026-11-02 创意方法入口增量

独立 feat/idea-method-entry，backend a18f72d7faa3430fa496a4912b9508c7820f7fdc、
web 9bc5f5afa80c1464875c9d3586489d2fde7c60c6，子仓均先推 GitHub。
创意存精确方法选择，页面从真实方法库选版并带到工作稿正式预览/绑定。
数据库 47 passed，前端 188 passed / 2 skipped，静态检查与 build 通过。
方法内容仍须作者接受，不把 20 个合成夹具当生产方法；当前 qwen 候选不改。
这是后续增量，须配同批前后端；旧前端无法编辑该字段，回退不可覆盖新创意。


## 2026-11-02 L4 数据格式贯通（后续增量，未部署）

backend c8d9a70617154204678f1adfab0867328293a16d，web 保留 9bc5f5a。
新增 knowbase-user-data-lock，钉本人格式响应；incubation v1 仅说明更新，字段不变，
knowbench 消费锁同批。作者挑选版本时核摘要与格式，保存快照随每轮孵化传模型。
59 项回归无跳过，含隔离数据库；ruff/pyright 通过。先部署 knowbase 格式接口；
不能将旧服务 404 当成功空格式。此项不等于方法作者接受或实战验收。


## 2026-11-03 Markdown 成稿交付

开发后端 37f5e6e，前端 aa95bef；编译模板按文件声明分流，作者高级格子可保存文件名。
100 项作者侧回归、运行侧 26 项文件检查与 21 项隔离数据库回归通过；前端189通过/2跳过，类型与定向 lint 通过。
从 #58 隔离的候选父 c0d8445 / 后 491538c / 前 a257754，分支 fix/markdown-delivery。
按所有者最新授权，已通过正式 KnowbotWorkdraftService.save 将财务专家工作稿29→30，
仅 steps/writeup/advanced.output_file=财务分析报告.md；事务内逐字段验证其余内容不变。
尚未生成/发布专家新版，须先部署编译修复再由 Luna 走测试/交付，用户不用重新手动孵化。
旧专家/任务未改；当前报告原样导出至用户财务演练项目，不能冒充旧任务列表自动登记。


## 2026-11-03 新版财务测试同步发布候选

基于#59，仅加入唯一行测试条件及配套前后端契约；同步补充采用状态、成稿完整性修复。旧用例不原地修改，旧请求不带row；新用例需两端部署后经正式revise/publish/start_self_test登记与执行。所有者已确认测试清单，线上工作稿33已准备，尚未创建新测试；T2语义、T3人数/缺口、T4授权轨迹仍须人工核对，不以字段检查通过冒充业务全验收。

## 2026-11-03 第二轨方法换版核对

前端e8ca90baaca96ae59125ca0a5ca866d0cd3d4784修复“列表版本2但选择仍为1”的误导，
增加原版与所选版本差异核对、原摘要失败阻断、新输入槽重连和重新预览要求。
全量198 passed/2 skipped，typecheck、定向eslint、i18n通过。子仓已推两远端。
仅开发父仓推进gitlink；Qwen的fix/test-row-selector父b29dea2及其全部候选不变。
无新方法内容、后端契约或生产记录变更；正式第二轨业务闭环仍待作者/数据验收。

## 2026-11-03 作者端动态成稿预检

后端ee8fe106c14e0e2801360d0015e4495c04da0bf8已推两远端。必经/最终方法及传递
输入的文件冲突在作者预览/装配/发布时指向具体格子，保留交回.md；可选组合由运行时判。
不改历史快照重放，不改固定专家；无迁移/新契约。10项先失败，修复后129项回归与
30项隔离DB通过，Ruff/Pyright通过。开发父仓推进，Qwen候选b29dea2保持不变。
