# knowlab 的契约

| 目录或文件 | 角色 | 说明 |
| --- | --- | --- |
| `knowbot-test-run/v1/` | **提供方（真源）** | 运行方跑测试的契约（SDD 0027 决定 12）。由后端 `app/domain/knowbots/test_run.py` 的模型生成：在 `knowlab-backend/app` 下 `python -m app.domain.knowbots.schema`；测试核文件与模型逐字一致。运行方实现它，入口统一为 `/api/internal/v1/knowbot-runtime/test-runs`。10-30 加接口 `POST …/test-runs/{run_id}/cancel`（作者撤回交付、审成不过时停平台重跑，回 RunResultV1；schema 不变，运行方待实现，没实现时 knowlab 照样撤授权、记平台出错） |
| `knowbot-incubation/v1/` | **提供方（真源）** | 运行方跑孵化的一轮（专家孵化方案第八节，decisions ⑮⑯）：作者点「AI 起草」或「让 AI 改…」，交过去动作与指令、创意原文、当前工作稿（格子与每格出处）、答过的缺口、作者挑的数据的格式（不含内容）、费用上限与作者的电脑；交回来出处表、缺口表、建议、自查。判断只出缺口、建议不碰工程层由模型守。同一个生成命令生成，真源 `app/domain/knowbots/incubation.py`，测试 `tests/test_knowbot_incubation_contract.py`。运行方入口 `/api/internal/v1/knowbot-runtime/incubations`（收、查、取消），服务关系 `knowbench:incubate` |
| `knowmart-knowbot-manifest-v2-lock.json` | 调用方（锁） | knowlab 向 knowmart 交付的分身清单 v2，真源在 `knowmart-app/contracts/knowbot/v2/`。锁只 pin major、schema 路径与 sha256；升级须提供方改 schema、两端测试都过，才更新锁。草稿只认 v2（decisions ⑰，10-28 起不留 v1） |
| `knowmart-delivery-v1-lock.json` | 调用方（锁） | knowlab 交一版专家给 knowmart 的请求 `knowbot-delivery/v1`（真源 `knowmart-app/contracts/knowbot-delivery/v1/`）：带平台人员审核时选的类目（knowmart 0032 待定第一项）。类目取值在 `app/domain/knowbots/delivery.py` 的 `CATEGORY_CODES`，`tests/test_knowbot_contract_locks.py` 核锁、类目与提供方一致、我们交的字段都在契约里；同一服务关系 `knowmart:deliver` 还调 `…/stop`（停用整版）与 `…/withdraw-public`（自用转公开撤回撞上已收下时只收回公开、保留自用） |

| `knowfeed-supply-status-lock.json` | 接收方（锁） | knowfeed 推需求状态 `knowfeed-supply-status/v1`（真源 `knowfeed-app/contracts/supply-status/v1/`，SDD 0045 第 2.5 节）。接收入口 `POST /api/internal/v1/knowfeed/demand-status`，服务关系 `supply-status`（scope `knowlab:supply-status`，只认 knowfeed）；收到后让等补采的测试马上重解析接着走。模型 `app/domain/knowbots/supply_status.py`，锁测试核摘要与结构 |
| `knowbase-data-request-lock.json` | 调用方（锁） | 一样数据的描述 `data-request/v1`，真源在 `knowbase-app/contracts/data-request/v1/`；测试时第 0 步按它提出要哪些数据、作者确认成测试集（块 7）。模型在 `app/domain/knowbots/data_request.py`，`tests/test_knowbot_contract_locks.py` 核锁与真源、本仓生成的 schema 与真源一致（不计说明文字）。10-28 起清单 v2 不再有数据需求声明（decisions ⑭），取代原 `knowbase-data-needs-lock.json` |

不在本仓放 knowmart schema 的副本。并列检出 `knowmart-app` 时，后端测试 `tests/test_knowbot_contract_locks.py` 核锁与提供方一致，并用提供方 schema 校验本仓的样例清单。

2026-11-02 WP-10 开发契约（尚未发布）：旧工作稿继续编译 v2；显式 `planning` 编译动态 `knowbot-manifest/v3`，新增 `knowmart-knowbot-manifest-v3-lock.json`。目录内部由 knowbench 提供 `contracts/dynamic-methods/v1/catalog.schema.json`，本仓以 `knowbench-dynamic-methods-lock.json` 锁定，`tests/test_dynamic_methods.py` 检查真实编译输出、摘要锁及导入往返。方法单元属于作者版本，任务计划不进清单。运行方执行入口在 WP-10 单元 3 完成前拒绝 v3，不能作为已可运行专家发布。

孵化请求 v1 的工作稿新增可选 `planning`、单元 `selection`，缺省序列化不变；建议/出处路径允许这两类作者格子。规划原文通过单独的严格模型校验，不开放单元 `method_text` 或 `advanced` 的 AI 改写。请求和结果 schema 与 knowbench 的孵化消费锁同步升级；作者本人接受之前，提议不能改工作稿。

10-28（decisions ⑰）删了 `knowbot-data/v1/`（运行方经 knowlab 读用户数据；数据改在 knowbase `user-data/v2`，测试集按用例签授权）与清单 v1 的锁 `knowmart-knowbot-manifest-lock.json`；`knowbot-test-run/v1` 的请求随之去掉 v1 测试运行授权 `data_grant`（用例的 `data` 照旧）。

10-28（decisions ⑯）测试都在作者的账号下跑：`knowbot-test-run/v1` 请求的 `runner` 改成必填，只有用户本人一种（`kind: "user"`，带电脑 `environment_id` 与工作区根目录 `workspace_root`）；原来缺省的「平台的测试账号与测试电脑」删了。运营发起的平台测试也带作者上次自助测试用过的电脑与目录。运行方的锁 `knowbench-app/contracts/knowlab-test-run-lock.json` 跟着更新。

WP-10 方法引用（开发契约，未发布）：工作稿 `method_refs` 由作者绑定 API 写入，普通编辑只读。引用清单使用 `dynamic-methods/v2`，`knowbench-dynamic-methods-v2-lock.json` 钉运行方 schema；`method_sources` 是独立主档的确切只读快照。导入/复制保留引用并核作者所有权，发布时回源验证。孵化请求可选引用字段及 knowbench 消费锁同步；原固定 v2 和动态目录 v1 schema 不变。

WP-10 动态测试产出（开发契约，未发布）：沿用 `knowbot-test-run/v1`。动态清单的用例期望
`artifact` 填作者 `planning.final_units` 的方法别名；不能填实例 ID 或内部 Artifact 名。
运行方按已接受计划及验收收据映射到确切成果，复用原 equals/approx/contains 判据。
提供方在建测试、签数据授权之前拒绝不匹配期望，运行方也校验；不是自动改作者断言。

仅 case-outputs schema 增加可选 `plan_version`、`final_outputs` 和步骤 `current`：
每份最终成果包含 method_id、instance_id、已验 result（id/version/digest，可为 null）、
受大小限制的 artifact、可选 deliverable 与 note。引用为 null 时不能带成果或文件。
结果摘要对应已验 JSON，不是回执中文件文字的指纹。没有当前有效成果时显式留空；
`current=false` 的历史 accepted 不能作为本次有效成果。总量超限可省内容但保留全部引用。
旧固定专家不序列化新字段；请求、结果、回答 schema 字节不变，消费锁只更新产出摘要。
作者测试页面应展示全部 final_outputs，兼容旧单文件，不能仅渲染 deliverable 判定无产出。
该接口扩展不解除 v3 外部准入门；作者页面和真实沙箱仍须联调验收。

L4 格式接入（2026-11-02 后续增量，未部署）：`knowbase-user-data-lock.json`
钉 user-data/v2 的 `MaterialFormatRead`。GET
`/api/internal/v2/user-data/materials/{material_id}/versions/{version_no}/format`
用作者本人令牌读取已展示给 knowlab、使用权覆盖该版本的表轴；核版本与摘要后
投影进既有 `DataFormat.tables`。CSV/XLSX 支持第一行/第一列标签，数据区数值与公式
不传；PDF、旧 xls、正文无表轴。坏文件/撤回展示/契约不符不能降级为空。
提供方先部署；`knowbot-incubation/v1` 只更新说明，消费锁摘要随之更新。
