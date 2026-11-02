# 2026-11-02 第二轨：WP-10 作者/运行契约

feat/dynamic-methods 已完成第一单元作者/运行契约，后端 dad9b0e20a8270747cfd999bacd7dafc3b2df6b0。显式 planning 与 selection 编译为 v3 动态目录；旧 v2 编译与摘要不变。作者确认方法单元、验收与工具边界，运行模型后续只能在冻结范围选择排序，不临时发明方法。计划存储/执行/UI 尚未实现，运行入口仍拒绝动态 v3。

父仓增加 knowmart v3 和 knowbench dynamic-methods/v1 消费锁，同步孵化 v1 请求/结果 schema（可选字段、planning/selection 建议路径）。三方样例来自真实编译器输出，无真实财务数据。完整本地回归 736 passed / 5 skipped，/tmp/luna-wp10-lab-full-final.txt；改动文件 lint 和域类型检查通过。全仓原有 1 个类型、11 个 lint 问题与解包基线逐条相同，无新增；见后端 CHECKPOINT.md。

下面是此前前置门的证据，不替代本次回归：

后端 70555a6：jsonschema 4.26.0 进入 dev 及锁，已有孵化/测试/清单 schema 校验不再 importorskip。补齐旧测试请求缺失的必填 runner，同时增加缺 runner 被拒的验证。此前省略 runner 的示例因缺依赖一直跳过，此次真实执行发现失败后修复，没有放宽 provider schema。

验证：`uv run --locked pytest -q tests/test_knowbot_workdraft_compile.py tests/test_knowbot_contract_locks.py tests/test_knowbot_incubation_contract.py tests/test_knowbot_test_run_contract.py`，71 passed；相关文件 ruff 通过；锁中已有运行依赖版本不变。输出 /tmp/luna-dynamic-contract-gates-lab-final.txt；初次失败保留于 /tmp/luna-dynamic-contract-gates-lab.txt。不是全仓/生产镜像/业务验收。

不改 qwen 正在发布的 fix/supply-delivery 候选；本分支未发布，不更新部署源，不调用 Wind。
