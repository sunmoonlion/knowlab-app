# knowlab 的契约

| 目录或文件 | 角色 | 说明 |
| --- | --- | --- |
| `knowbot-test-run/v1/` | **提供方（真源）** | 运行方跑测试的契约（SDD 0027 决定 12）。由后端 `app/domain/knowbots/test_run.py` 的模型生成：在 `knowlab-backend/app` 下 `python -m app.domain.knowbots.schema`；测试核文件与模型逐字一致。运行方实现它，入口统一为 `/api/internal/v1/knowbot-runtime/test-runs` |
| `knowbot-data/v1/` | **提供方（真源）** | 运行方读分身绑定的用户数据（SDD 0027 §3.5）。由后端 `app/domain/knowbots/data_read.py` 的模型生成：在 `knowlab-backend/app` 下 `python -m app.domain.knowbots.data_schema`；测试核文件与模型逐字一致。入口 `/api/internal/v1/knowlab/knowbot-data/{list,read}`，服务身份 `knowlab:read` |
| `knowmart-knowbot-manifest-lock.json` | 调用方（锁） | knowlab 向 knowmart 交付的分身清单外层，真源在 `knowmart-app/contracts/knowbot/v1/`。锁只 pin major、schema 路径与 sha256；升级须提供方改 schema、两端测试都过，才更新锁 |

不在本仓放 knowmart schema 的副本。并列检出 `knowmart-app` 时，后端测试 `tests/test_knowbot_contract_locks.py` 核锁与提供方一致，并用提供方 schema 校验本仓的样例清单；也可用 `KNOWMART_KNOWBOT_MANIFEST_SCHEMA_PATH` 指定提供方 schema。
