# knowlab 的契约

| 目录或文件 | 角色 | 说明 |
| --- | --- | --- |
| `knowbot-test-run/v1/` | **提供方（真源）** | 运行方跑测试的契约（SDD 0027 决定 12）。由后端 `app/domain/knowbots/test_run.py` 的模型生成：在 `knowlab-backend/app` 下 `python -m app.domain.knowbots.schema`；测试核文件与模型逐字一致。运行方实现它，入口统一为 `/api/internal/v1/knowbot-runtime/test-runs` |
| `knowmart-knowbot-manifest-v2-lock.json` | 调用方（锁） | knowlab 向 knowmart 交付的分身清单 v2，真源在 `knowmart-app/contracts/knowbot/v2/`。锁只 pin major、schema 路径与 sha256；升级须提供方改 schema、两端测试都过，才更新锁。草稿只认 v2（decisions ⑰，10-28 起不留 v1） |
| `knowbase-data-request-lock.json` | 调用方（锁） | 一样数据的描述 `data-request/v1`，真源在 `knowbase-app/contracts/data-request/v1/`；测试时第 0 步按它提出要哪些数据、作者确认成测试集（块 7）。模型在 `app/domain/knowbots/data_request.py`，`tests/test_knowbot_contract_locks.py` 核锁与真源、本仓生成的 schema 与真源一致（不计说明文字）。10-28 起清单 v2 不再有数据需求声明（decisions ⑭），取代原 `knowbase-data-needs-lock.json` |

不在本仓放 knowmart schema 的副本。并列检出 `knowmart-app` 时，后端测试 `tests/test_knowbot_contract_locks.py` 核锁与提供方一致，并用提供方 schema 校验本仓的样例清单。

10-28（decisions ⑰）删了 `knowbot-data/v1/`（运行方经 knowlab 读用户数据；数据改在 knowbase `user-data/v2`，测试集按用例签授权）与清单 v1 的锁 `knowmart-knowbot-manifest-lock.json`；`knowbot-test-run/v1` 的请求随之去掉 v1 测试运行授权 `data_grant`（用例的 `data` 照旧）。

10-28（decisions ⑯）测试都在作者的账号下跑：`knowbot-test-run/v1` 请求的 `runner` 改成必填，只有用户本人一种（`kind: "user"`，带电脑 `environment_id` 与工作区根目录 `workspace_root`）；原来缺省的「平台的测试账号与测试电脑」删了。运营发起的平台测试也带作者上次自助测试用过的电脑与目录。运行方的锁 `knowbench-app/contracts/knowlab-test-run-lock.json` 跟着更新。
