# 正式方法定义契约（开发候选，尚未开放）

提供方是 KnowLab。模型真源为后端 `formal_method.py`，结构编译在
`formal_method_compiler.py`；Schema 和消费代码由程序生成。

作者只维护能力声明、所需权限、做法、校验及审批五项。直接实现声明输入输出；编排步骤
每次选择一个精确版本的方法，通过封闭的传值规则绑定输入，用明确导出映射交付结果。
输入与返回视图从定义生成，不接收另一份可编辑视图。

分类检查全部允许候选，按专家方法 ID 去重，不按版本或调用次数计数；专家团依赖向上
传播。循环、展开、深度上限分别检查，正常展开不消耗纠错重排额度。

编译结果只证明结构一致。源资产归属、来源与许可、实际测试证据、冻结、发布资格和运行
授权必须由应用服务另行核验。不能将此 Schema 校验通过等同于孵化通过。本候选尚未接通
作者工作稿、孵化测试、冻结和运行派发，现有作者入口限制继续生效。

在 `knowlab-backend/app` 中生成：

```sh
.venv/bin/python -m app.domain.knowbots.schema
.venv/bin/python -m scripts.export_formal_method --consumer ../../../knowbench-app/knowbench-backend/app
```

检查消费者没有漂移时，给第二条命令加 `--check`。消费端文件记录源代码及 Schema 摘要。
本契约没有旧方法格式的兼容转换器；切换作者及运行入口须作为同批迁移完成。
