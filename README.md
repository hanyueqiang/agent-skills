# Agent Skills

[简体中文](./README.zh-CN.md)

Small, focused skills for coding agents.

## Skills

| Skill | Purpose |
| --- | --- |
| [`frontend-change-scout`](./frontend-change-scout/) | Map the smallest safe frontend change before implementation. |

## Install

Copy the skill directory into a Codex project's `.codex/skills/` directory, or into the user-level `~/.codex/skills/` directory:

```text
.codex/skills/frontend-change-scout/
├── SKILL.md
└── agents/openai.yaml
```

## Use

Ask explicitly for the skill when you want predictable activation:

```text
Use $frontend-change-scout to map the smallest safe change for this frontend request. Do not edit code.
```

The skill reports the relevant route, components, state/data flow, styling, tests, constraints, and the smallest validation command. It performs reconnaissance only; implementation remains a separate request.
