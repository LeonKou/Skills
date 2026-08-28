<div align="center">

# LeonKou Skills

### Engineering context for AI coding agents.

把系统设计、工程规范和开发上下文，变成编码智能体真正能发现并遵守的项目资产。

[![GitHub stars](https://img.shields.io/github/stars/LeonKou/Skills?style=flat-square&color=b7ff3c&labelColor=070908)](https://github.com/LeonKou/Skills/stargazers)
[![License](https://img.shields.io/badge/license-MIT-b7ff3c?style=flat-square&labelColor=070908)](LICENSE)
[![Pages](https://img.shields.io/website?url=https%3A%2F%2Fleonkou.github.io%2FSkills%2F&style=flat-square&label=docs)](https://leonkou.github.io/Skills/)

[产品主页](https://leonkou.github.io/Skills/) · [快速开始](#快速开始) · [贡献](#贡献)

</div>

## 为什么是 Skills

AI 可以很快写出代码，但它不会自动知道你的系统边界、接口契约和团队约定。这个仓库把这些隐性知识变成可版本化、可追踪、可复用的 Skills，让人类和 AI 在同一份工程上下文中协作。

## 当前 Skill

### [`system-blueprint`](skills/system-blueprint/README.md)

通过 15 个阶段的引导式提问，将模糊需求渐进式编译成完整系统蓝图：

- 业务目标、角色、用例、领域模型、状态和流程；
- 前后端工程与业务模块的一一对应；
- OpenAPI 与 JSON Schema 接口契约；
- 数据库、错误码、配置和部署规范；
- `AGENTS.md`、`SYSTEM_BLUEPRINT.md`、`module-map.yaml` 等 AI 发现入口；
- 需求演进、冲突确认、版本记录和质量门禁。

## 快速开始

```bash
git clone https://github.com/LeonKou/Skills.git
cp -R Skills/skills/system-blueprint .agents/skills/system-blueprint
```

Windows PowerShell：

```powershell
git clone https://github.com/LeonKou/Skills.git
Copy-Item -Recurse .\Skills\skills\system-blueprint .agents\skills\system-blueprint
```

然后在项目中提出：

> 帮我为这个系统创建完整的软件系统蓝图，并按阶段逐项确认。

Skill 会检查已有项目约定，逐阶段提问，并在每次确认后保存工作状态。

## 设计承诺

| 承诺 | 含义 |
|---|---|
| Confirmation-first | 未经确认的内容不会变成正式规范 |
| Domain-first | 前后端都按业务领域组织，快速定位对应代码 |
| Contract-first | API Schema 和数据约束先于实现 |
| Traceable | 需求、设计、实现和测试互相追踪 |
| Recoverable | 中断后从工作状态继续，不丢失上下文 |
| Human-controlled | 通用规范回写 Skill 必须经过用户确认 |

## 目录结构

```text
skills/
└── system-blueprint/
    ├── SKILL.md
    ├── README.md
    ├── references/
    ├── templates/
    └── evals/
docs/
└── index.html
.github/workflows/
└── deploy-pages.yml
```

## 依赖策略

Skill 默认无运行时依赖。Skill 之间如有依赖，必须在自身文档中声明名称、版本和用途；调用前检查依赖是否可用，缺失时明确说明并降级，不静默修改其他 Skill。项目蓝图规则优先于 Skill 间建议。

## GitHub Pages

文档站点使用仓库根目录 `docs/`，由 GitHub Actions 自动部署。访问 [leonkou.github.io/Skills](https://leonkou.github.io/Skills/) 查看产品化介绍。

## 贡献

欢迎提交新的 Skill、模板和评测用例。请确保：

1. 说明触发范围和适用场景；
2. 提供安装和依赖说明；
3. 规范区分强制要求与背景建议；
4. 变更包含兼容性说明和验证方式；
5. 不提交密钥、Token 或敏感数据。

## License

MIT
