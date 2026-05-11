# Superpowers Skills

> 跨 AI 代码客户端的后端服务开发规范技能库

## 支持的客户端

| 客户端 | 配置文件 | 加载方式 |
|--------|----------|----------|
| Claude Code | `CLAUDE.md` | 自动加载项目根目录的 CLAUDE.md |
| OpenCode | `SKILL.md` + skill tool | 使用 skill 工具加载 |
| Codex | `cody.json` + 目录 | Cody 插件自动识别 `.cody/` |

## 技能列表

### Engineering

| 技能 | 说明 |
|------|------|
| [backend-dev](/skills/engineering/backend-dev/SKILL.md) | 后端开发规范（API设计、数据库、响应格式、工程结构） |

## 使用方式

### Claude Code

将 `CLAUDE.md` 复制到你的项目根目录即可。

### OpenCode

使用 skill 工具加载：
```
/skill china-stock-analysis
```

### Codex

Cody 会自动从 `.cody/` 目录加载技能。

## 目录结构

```
skills/
├── README.md                      # 本文件
├── CONTEXT.md                     # 术语表
└── engineering/                   # 工程技能
    └── backend-dev/
        └── SKILL.md              # 后端开发规范
```

## License

MIT