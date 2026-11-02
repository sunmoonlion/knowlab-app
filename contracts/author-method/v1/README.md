# 作者方法定义 v1（开发契约）

提供方是真源 knowlab。`definition.schema.json` 由后端 `domain/knowbots/methods.py` 生成；不是专家运行包，也不是任务计划。当前只用于作者主档及引用预览，尚未交给 knowmart/knowbench 消费，不得当作跨应用集成已完成。

在 `knowlab-backend/app` 运行：

```sh
uv run --locked python -m app.domain.knowbots.methods > ../../contracts/author-method/v1/definition.schema.json
```

定义复用已有作者格子。方法内的标识固定为 `method`，`uses`、覆盖检查来源与回退目标使用命名输入槽，`input_schemas` 记冻结时的上游输出结构。正文、规则、检查、工具与调用上限保留作者已确认内容；是否必经由专家组合决定，定义中的 `selection.required` 固定为 false。迁移结构引用不会语义改写原文；原文若仍写「第几步」等上下文，须作者改稿、确认后另冻版本。

主档身份、版本号、摘要、来源工作稿/修订及作者证据由数据库保存，客户端不能提交任意定义来跳过出处核验。摘要按 UTF-8、键排序、紧凑 JSON 计算并加 `sha256:` 前缀；别名、输入连接与当前任务不写进方法定义。专家引用还须钉主档 ID 与版本，不能只靠摘要或 latest。

现有 web 会话与 CSRF 下的开发接口：

- `PUT /api/web/v1/method-units/{UUID}/versions/{n}`：本人确认来源工作稿、修订、单元与原输入别名到命名槽的映射；首次版本 1，之后连续递增。同版本同请求幂等，换载荷 409；旧版本不覆盖。
- `GET /api/web/v1/method-units`：仅本人，limit 1～100、offset 分页；latest_version 只作列表提示，不代表自动升级引用。
- `GET /api/web/v1/method-units/{UUID}/versions/{n}`：读取确切版本；不是本人与不存在同为 404。
- `POST /api/web/v1/method-units/{UUID}/versions/{n}/preview-binding`：给出目标工作稿/修订、别名、摘要、输入连接与调用范围，检查输出结构、图、必需成果和工具边界；只回候选，`preview_only=true`，不存引用、不发专家版本、不启动任务。

真实保存专家引用、升级确认、编译清单与运行方校验仍待下一段实现。前端编辑/选择入口也未接；不要通过复制预览里的 step JSON 冒充已保存精确版本引用。方法主档只由 knowlab 编辑，后续消费者仅持稳定引用和只读快照。
