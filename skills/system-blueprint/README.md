# system-blueprint

面向开发者和 AI 编码智能体的系统蓝图设计引导 Skill。它通过 15 个阶段的渐进式访谈，生成可执行、可追踪、可恢复，并能约束后续开发的项目蓝图。

## 让 AI 帮你安装

最简单的方式是把下面这段话直接发送给 Codex、Claude Code、OpenCode、Pi 或其他支持文件操作的编码 Agent：

```text
请从 https://github.com/LeonKou/Skills/tree/main/skills/system-blueprint 安装 system-blueprint Skill。
先识别你当前支持的项目级 Skill 目录；复制完整的 system-blueprint 目录并保留 SKILL.md、references、templates 和 evals。
优先安装到当前项目，使它可以随项目版本控制；安装后验证 Skill 可被发现，并告诉我实际安装路径和验证结果。不要覆盖同名目录，发现已有版本时先比较并征求确认。
```

若希望对所有项目生效，把“优先安装到当前项目”改成“安装到当前用户的全局 Skill 目录”。

## 安装位置

必须复制整个 `system-blueprint/` 目录，不能只复制 `SKILL.md`，因为 Skill 依赖随附的 `references/` 和 `templates/`。

| Agent | 项目级安装（推荐） | 用户级安装 |
|---|---|---|
| Codex | `.agents/skills/system-blueprint/` | `~/.agents/skills/system-blueprint/` |
| Claude Code | `.claude/skills/system-blueprint/` | `~/.claude/skills/system-blueprint/` |
| OpenCode | `.opencode/skills/system-blueprint/` 或 `.agents/skills/system-blueprint/` | `~/.config/opencode/skills/system-blueprint/` 或 `~/.agents/skills/system-blueprint/` |
| Pi | `.pi/skills/system-blueprint/` 或 `.agents/skills/system-blueprint/` | `~/.pi/agent/skills/system-blueprint/` 或 `~/.agents/skills/system-blueprint/` |

### 跨 Agent 项目推荐

需要同一个项目同时供 Codex、OpenCode 和 Pi 使用时，统一安装到：

```text
.agents/skills/system-blueprint/
```

Claude Code 使用自己的项目目录。若项目也需要 Claude Code，复制或链接同一目录到：

```text
.claude/skills/system-blueprint/
```

不要维护两份内容不同的 Skill；应确定一个来源并同步更新。

## 命令行安装

以下命令应在目标项目根目录运行。

### macOS / Linux

```bash
git clone --depth 1 https://github.com/LeonKou/Skills.git .skills-source
mkdir -p .agents/skills
cp -R .skills-source/skills/system-blueprint .agents/skills/system-blueprint
```

Claude Code 项目级安装：

```bash
mkdir -p .claude/skills
cp -R .skills-source/skills/system-blueprint .claude/skills/system-blueprint
```

### Windows PowerShell

```powershell
git clone --depth 1 https://github.com/LeonKou/Skills.git .skills-source
New-Item -ItemType Directory -Force .agents\skills | Out-Null
Copy-Item -Recurse .skills-source\skills\system-blueprint .agents\skills\system-blueprint
```

Claude Code 项目级安装：

```powershell
New-Item -ItemType Directory -Force .claude\skills | Out-Null
Copy-Item -Recurse .skills-source\skills\system-blueprint .claude\skills\system-blueprint
```

确认安装完成后，可以删除临时的 `.skills-source`。若目标目录已经存在，不要直接覆盖；先比较版本和本地修改。

## 各 Agent 使用说明

### Codex

Codex 会扫描从当前工作目录到仓库根目录之间的 `.agents/skills/`。把 Skill 提交到项目仓库，团队成员在该仓库中启动 Codex 后即可共同使用。

安装后可提问：

```text
请列出当前可用的 Skills，并使用 system-blueprint 为这个项目创建系统蓝图。
```

### Claude Code

Claude Code 使用 `.claude/skills/`（项目级）或 `~/.claude/skills/`（用户级）。项目级 Skill 可以提交到版本库；首次使用项目资源时按客户端提示确认工作区信任。

安装后可输入：

```text
/system-blueprint
```

也可以用自然语言触发：

```text
请使用 system-blueprint，按阶段引导我完成这个系统的正式蓝图。
```

### OpenCode

OpenCode 支持 `.opencode/skills/`、`.agents/skills/` 和 Claude 兼容目录。建议单工具项目使用 `.opencode/skills/`，跨 Agent 项目使用 `.agents/skills/`。

验证提示：

```text
请确认 system-blueprint Skill 已被发现，然后从蓝图阶段 1 开始。
```

### Pi

Pi 支持 `.pi/skills/`、`.agents/skills/`、`~/.pi/agent/skills/` 和 `~/.agents/skills/`。首次加载包含项目级资源的目录时，Pi 可能要求确认项目可信。

验证提示：

```text
检查 system-blueprint 是否可用，并读取它的完整工作流后开始访谈。
```

### 其他 Agent

对于支持 [Agent Skills](https://agentskills.io/) 目录规范的工具，优先尝试 `.agents/skills/system-blueprint/`。如果工具没有原生 Skills 功能，让它直接读取本目录的 `SKILL.md`，并在项目 `AGENTS.md` 中声明开发任务开始前必须遵守该 Skill。

不要假设所有工具使用同一个全局目录；全局安装前应先查看该工具当前版本的官方文档。

## 安装验证

安装后检查：

```text
<agent-skill-directory>/system-blueprint/
├── SKILL.md
├── README.md
├── references/
├── templates/
└── evals/
```

然后向 Agent 提问：

```text
system-blueprint 在什么场景触发？请只概述工作流程，不要开始创建文件。
```

回答应包含“15 个阶段”“逐项确认”“持久化”“正式版质量门禁”等核心概念。若 Agent 找不到 Skill：

1. 检查目录名和 `SKILL.md` 大小写；
2. 确认 `SKILL.md` 位于 `system-blueprint/` 根部；
3. 重启或重新加载 Agent 会话；
4. 确认启动目录处于包含项目级 Skill 的仓库内；
5. 检查工作区信任和 Skill 权限设置。

## 项目使用

在项目根目录触发 Skill。首次确认后，它会生成 `AGENTS.md`、`SYSTEM_BLUEPRINT.md`、`module-map.yaml` 和 `docs/system-blueprint/`。将这些文件提交到版本库，使所有编码智能体共享同一开发基线。

## 依赖处理

本 Skill 无运行时依赖。`references/`、`templates/` 和 `evals/` 是随 Skill 分发的资源。多个 Skill 互相依赖时，在各自 `SKILL.md` 中声明依赖名、版本、用途和失败策略；缺失依赖时明确说明，不静默复制、覆盖或修改其他 Skill。

## 更新与回写

项目专属规则只更新项目蓝图。跨项目通用规范先形成回写提案，必须得到用户明确确认后才修改 Skill。升级已安装的 Skill 前先比较本地修改，避免覆盖用户定制内容。

## 官方参考

- [Codex Skills](https://learn.chatgpt.com/docs/build-skills)
- [Claude Code Skills](https://code.claude.com/docs/en/skills)
- [OpenCode Agent Skills](https://opencode.ai/docs/skills/)
- [Pi coding agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent)
- [Agent Skills specification](https://agentskills.io/)

## License

[0BSD](../../LICENSE)
