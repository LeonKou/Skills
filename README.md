# LeonKou Skills

> 面向 AI 编码智能体的工程化技能库：把设计决策、开发规范与可执行工作流固化进代码库。

## 为什么是这个仓库

工程团队不把约定藏在聊天记录里。本仓库将可复用的 Skills、规范模板和自动化工作流版本化，让 Codex、Claude Code、Vibe Coding 工具与开发者共享同一套上下文。

## Skills

| Skill | 用途 | 状态 |
|---|---|---|
| [system-blueprint](skills/system-blueprint/README.md) | 通过 15 阶段引导生成可执行系统蓝图、模块映射和开发规范基线 | stable |

## 安装

```bash
git clone https://github.com/LeonKou/Skills.git
cp -R Skills/skills/system-blueprint .agents/skills/system-blueprint
```

Windows PowerShell：

```powershell
Copy-Item -Recurse .\Skills\skills\system-blueprint .agents\skills\system-blueprint
```

安装后重新加载编码工具。中文提到“系统蓝图、架构设计、开发规范基线、需求到代码约束”等场景时即可触发。

## 设计原则

- 中文引导，英文稳定标识；
- 渐进式提问，用户确认后持久化；
- 领域优先，前后端工程内镜像对应；
- 契约先于实现，需求到测试可追踪；
- 只把确认后的强制规则写入规范；
- 项目规则优先于 Skill 默认建议。

## 文档与站点

- [产品宣传页](https://leonkou.github.io/Skills/)
- [system-blueprint Skill](skills/system-blueprint/SKILL.md)
- [安装和依赖说明](skills/system-blueprint/README.md)

## 贡献

请提交清晰的变更说明、适用范围和验收用例。涉及通用规范的变更必须说明兼容性影响；项目专属规则应留在项目蓝图中。

## License

MIT
