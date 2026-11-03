# author-method/v2

提供方：knowlab。`definition.schema.json` 由 `methods.render(optional=True)` 生成。

在独立方法正文、输入形状之外，新增完整的 `input_required` 映射。每个独立输入槽必须声明严格布尔值；方法引用不得改变该值。条件检查使用 `when_input_present`，仅指向可选槽；covers 必须与自身数据来源使用相同条件，必需输出检查不可条件化。

定义仍使用中立的 `method` 身份、独立槽名和内容摘要。作者在专家工作稿装配时绑定上游别名，冻结/导入/引用均保留条件与精确版本。旧 definition/v1 的生成文件、摘要、输入语义保持不变。
