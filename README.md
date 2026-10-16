# knowlab-app

**knowlab**（知识实验室）：用户提供想法（最好是 skill）与自己的数据，我们在这里把它工程化、测试好，交付成用户自己的**分身**（knowbot）。投资逻辑与数据都来自用户，我们只做工程化与测试；分身只归用户自己，交付后存进 knowmart，用户在 investment 工作台里「请」来用。

由 [tpl-app](https://github.com/sunmoonlion/tpl-app) 实例化（tpl-app 4b19daf，2026-10-16）：

```text
knowlab-app/
├── knowlab-backend/          FastAPI 后端（Admin / Web / Internal 三面一个工程）
├── knowlab-web-frontend/     Next.js 网页端
└── knowlab-admin-frontend/   Next.js 管理端
```

设计、规则、部署都在并列的 `k8s` 仓：先读 `../k8s/docs/project-guide/overall-architecture.md`，本应用的设计见 `../k8s/docs/dev-investment-agent/tree-build/SDD/modules/0026-knowlab.md`；动代码前读 `../k8s/docs/dev-investment-agent/tree-build/rules/constraints.md`（含合规 `C-L*`）。构建与部署由 k8s 仓的发布链做，本仓不放部署脚本。

**现在只是空应用**：模板的骨架、认证与运行角色，没有业务功能。
