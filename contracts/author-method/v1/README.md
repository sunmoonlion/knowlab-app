# 作者方法定义 v1（开发契约）

提供方是真源 knowlab。`definition.schema.json` 由后端 `domain/knowbots/methods.py` 生成；不是专家运行包，也不是任务计划。已有作者主档、预览及精确引用保存，编译清单以 dynamic-methods/v2 携带只读定义快照，knowmart/knowbench 按消费锁核验。动态 v3 运行准入仍关闭，不能据此声明生产可用。

在 `knowlab-backend/app` 运行：

```sh
uv run --locked python -m app.domain.knowbots.methods > ../../contracts/author-method/v1/definition.schema.json
```

定义复用已有作者格子。方法内的标识固定为 `method`，`uses`、覆盖检查来源与回退目标使用命名输入槽，`input_schemas` 记冻结时的上游输出结构。正文、规则、检查、工具与调用上限保留作者已确认内容；是否必经由专家组合决定，定义中的 `selection.required` 固定为 false。迁移结构引用不会语义改写原文；原文若仍写「第几步」等上下文，须作者改稿、确认后另冻版本。

主档身份、版本号、摘要、来源工作稿/修订及作者证据由数据库保存，客户端不能提交任意定义来跳过出处核验。摘要按 UTF-8、键排序、紧凑 JSON 计算并加 `sha256:` 前缀；别名、输入连接与当前任务不写进方法定义。专家引用还须钉主档 ID 与版本，不能只靠摘要或 latest。

现有 web 会话与 CSRF 下的开发接口：

- `PUT /api/web/v1/method-units/{UUID}/versions/{n}`：本人确认来源工作稿、修订、单元与原输入别名到命名槽的映射；首次版本 1，之后连续递增。同版本同请求幂等，换载荷 409；旧版本不覆盖。
- `GET /api/web/v1/method-units`：仅本人，limit 1～100、offset 分页，回 `{items: [...]}`；项含 method_id、latest_version、created_at、最新版本的 title 和截到 500 字的 summary。标题与概要由同一次查询读确切最新版本，不在前端维护行业方法表；它们只作选择提示，仍须精确读版。latest_version 不代表自动升级引用。
- `GET /api/web/v1/method-units/{UUID}/versions/{n}`：读取确切版本；不是本人与不存在同为 404。
- `POST /api/web/v1/method-units/{UUID}/versions/{n}/preview-binding`：给出目标工作稿/修订、别名、摘要、输入连接与调用范围，检查输出结构、图、必需成果和工具边界；只回候选，`preview_only=true`，不存引用、不发专家版本、不启动任务。
- `POST /api/web/v1/method-units/{UUID}/versions/{n}/bind`：同一 BindMethod 请求，作者明确确认后保存引用，返回完整 Workdraft。正文和确切版本不可由普通格子保存改写；复用现有来源记录及 revision 冲突，既不发布专家，也不启动运行。

BindMethod 的 request_id 是目标工作稿所属申请 ID；revision、alias、digest、inputs、
required、max_calls 均按实际模型提供，最后两项没有缺省值。冻结版本使用来源申请及修订、
step_id 和原方法别名到独立槽名的 input_slots，不能上传任意定义或伪造出处来跳过核验。

动态目录顺序不是任务执行顺序，候选输入可指向目录中任何其它方法；服务检查整图及
类型/循环边界。引用精确版本不自动跟随主档升级。方法主档只由 knowlab 编辑，消费者
仅持稳定引用和只读快照。普通测试不应修改版本管理中的样例文件来伪造契约一致。
