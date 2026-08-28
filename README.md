# Superpowers Skills

> 跨 AI 代码客户端的后端服务开发规范技能库

## 支持的客户端

| 客户端 | 加载方式 |
|--------|----------|
| Claude Code | 自动加载项目根目录的 `CLAUDE.md` |
| OpenCode | 使用 `/skill` 命令加载 |
| Codex | Cody 插件自动识别 `.cody/` 目录 |

## 技能列表

### Engineering

| 技能 | 说明 |
|------|------|
| [backend-dev](skills/engineering/backend-dev/SKILL.md) | 后端开发规范（API设计、数据库、响应格式、工程结构） |

## 安装方式

### Claude Code

复制 `CLAUDE.md` 和 `skills/` 到项目根目录。

### OpenCode

```bash
/skill add /path/to/skills
```

### system-blueprint

`skills/system-blueprint/` 是面向中文开发者和 AI 编码智能体的系统蓝图设计引导器。将该目录随仓库提交即可使用；详细安装、依赖处理、项目文件契约和 GitHub Pages 说明见 [system-blueprint/README.md](skills/system-blueprint/README.md)。

该 Skill 不依赖运行时包。若 Skill 之间存在依赖，应在各自 `SKILL.md` 声明依赖名、版本和用途；调用前检查依赖是否可用，缺失时说明并降级，不静默修改其他 Skill。

## 目录结构

```
skills/
├── CLAUDE.md                      # Claude Code 配置
├── README.md                      # 本文件
├── CONTEXT.md                     # 术语表
└── engineering/                   # 工程技能
    └── backend-dev/
        └── SKILL.md              # 后端开发规范

## License

MIT
