# 按列值选择测试行

`Expectation.row` 是可选扩展。`path` 仍先定位交回物；带 `row` 时该位置必须是列表。
`where` 的所有列值必须同时匹配，且恰好匹配一行，再按 `field` 点号路径取值，
最后执行原 `equals` / `approx` / `contains` 比较。

```json
{
  "artifact": "indicators",
  "path": "annual_metrics",
  "row": {"where": {"year": "2024"}, "field": "revenue"},
  "kind": "approx",
  "value": 100,
  "tolerance": 0.001
}
```

上例金额是合成测试值。选行不依赖列表顺序，缺行、重复行、缺字段都失败，不退回第一行。
where 为1～8个列名与标量值；布尔不等于数字，文字不强制转数字，整数与等值浮点数可匹配。
非法形状、空列名、无限数在请求入口拒绝。原比较与相对容差语义不变。

不使用row时不序列化该字段，旧用例与运行请求摘要形状不变。新字段不能发给旧运行端：
provider与consumer的严格DTO须同版发布，双方schema/hash锁一致后才登记或启动新测试。
旧记录、旧专家、旧任务不改写。现有超时、重试、run_id幂等与结果回收机制不变。

该能力只作确定性字段比较，不能替代对数据授权、引文语义、财务口径的人工验收。
