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

10-28（decisions ⑰）删了 `knowbot-data/v1/`（运行方经 knowlab 读用户数据；数据改在 knowbase `user-data/v2`，测试集按用例签授权）与清单 v1 的锁 `knowmart-knowbot-manifest-lock.json`；`knowbot-test-run/v1` 的请求随之去掉 v1 测试运行授权 `data_grant`（用例的 `data` 照旧）。

10-28（decisions ⑯）测试都在作者的账号下跑：`knowbot-test-run/v1` 请求的 `runner` 改成必填，只有用户本人一种（`kind: "user"`，带电脑 `environment_id` 与工作区根目录 `workspace_root`）；原来缺省的「平台的测试账号与测试电脑」删了。运营发起的平台测试也带作者上次自助测试用过的电脑与目录。运行方的锁 `knowbench-app/contracts/knowlab-test-run-lock.json` 跟着更新。
