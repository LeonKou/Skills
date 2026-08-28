# system-blueprint

面向中文开发者和 AI 编码智能体的系统蓝图设计引导 Skill。它通过 15 个阶段的渐进式访谈，生成可执行的项目蓝图与开发规范基线。

## 安装

### Codex / Claude Code

将本目录复制到对应的 skills 目录（例如 `$CODEX_HOME/skills/system-blueprint` 或项目级 `.agents/skills/system-blueprint`），保持 `SKILL.md` 位于目录根部。重新加载工具后，提到“系统蓝图”“架构设计”“开发规范基线”等中文请求即可触发。

### 项目使用

在项目根目录运行 Skill，首次确认后生成 `AGENTS.md`、`SYSTEM_BLUEPRINT.md` 和 `docs/system-blueprint/`。提交这些文件到版本库，让所有编码智能体共享同一基线。

## 依赖处理

本 Skill 无运行时依赖；`references/` 和 `templates/` 是随 Skill 分发的资源。若多个 Skill 互相依赖，在各自 `SKILL.md` 的“依赖”段声明依赖名、版本和用途；调用前检查依赖是否可用，缺失时说明并降级，不复制或静默修改其他 Skill。项目规则优先于 Skill 间建议，冲突按蓝图中的优先级协议处理。

## GitHub Pages

仓库根目录可使用 `docs/` 作为 Pages 发布目录。本 Skill 提供 `docs/index.md`（由仓库维护者生成或调整），并建议在 GitHub Settings → Pages 中选择 `Deploy from a branch`、目标分支的 `/docs`。Pages 只展示文档，不承载密钥或私有数据。

## 更新与回写

项目专属规则只更新项目蓝图。跨项目通用规范先生成回写提案，必须得到用户明确确认后才修改 Skill；每次变更更新版本和变更日志。
