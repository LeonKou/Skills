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