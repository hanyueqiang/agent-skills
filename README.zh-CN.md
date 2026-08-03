# Agent Skills

[English](./README.md)

面向编程 Agent 的小而专注的技能集合。

## Skills

| Skill | 用途 |
| --- | --- |
| [`frontend-change-scout`](./frontend-change-scout/) | 在开始实现前，绘制最小且安全的前端改动地图。 |
| [`design-system-reuse`](./design-system-reuse/) | 在创建 UI 前复用现有设计系统的组件与规范。 |

## 安装

将所需 skill 目录复制到 Codex 项目的 `.codex/skills/`，或用户级的 `~/.codex/skills/`：

```text
.codex/skills/frontend-change-scout/
├── SKILL.md
└── agents/openai.yaml
```

## 使用

需要稳定命中时，请显式提及 skill：

```text
使用 $frontend-change-scout，为这个前端需求绘制最小且安全的改动地图。不要修改代码。
```

它会输出相关路由、组件、状态/数据流、样式、测试、约束与最小验证命令。该 skill 只进行侦察；实现代码需在后续请求中单独执行。
