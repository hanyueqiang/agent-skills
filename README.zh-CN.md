# Agent Skills

[English](./README.md)

面向编程 Agent 的小而专注的技能集合。

## Skills

| Skill | 用途 |
| --- | --- |
| [`frontend-change-scout`](./frontend-change-scout/) | 在开始实现前，绘制最小且安全的前端改动地图。 |
| [`design-system-reuse`](./design-system-reuse/) | 在创建 UI 前复用现有设计系统的组件与规范。 |
| [`legacy-mobile-style-compat`](./legacy-mobile-style-compat/) | 让 UI 样式兼容较旧的移动浏览器与 WebView。 |

## 安装

将单个 skill 目录复制到所使用编程 Agent 对应的目录。

### Codex

```text
.codex/skills/<skill-name>/
└── SKILL.md
```

用户级 Codex 安装路径为 `~/.codex/skills/<skill-name>/`。

### Claude Code

```text
.claude/skills/frontend-change-scout/
└── SKILL.md
```

用户级 Claude Code 安装路径为 `~/.claude/skills/frontend-change-scout/`。本仓库采用可移植的 Agent Skills 格式；`agents/openai.yaml` 是 Codex 可选元数据，Claude Code 不需要它。

## 使用

需要稳定命中时，请显式提及 skill：

```text
使用 $frontend-change-scout，为这个前端需求绘制最小且安全的改动地图。不要修改代码。
```

它会输出相关路由、组件、状态/数据流、样式、测试、约束与最小验证命令。该 skill 只进行侦察；实现代码需在后续请求中单独执行。
