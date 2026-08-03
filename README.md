# Agent Skills

[简体中文](./README.zh-CN.md)

Small, focused skills for coding agents.

## Skills

| Skill | Purpose |
| --- | --- |
| [`frontend-change-scout`](./frontend-change-scout/) | Map the smallest safe frontend change before implementation. |
| [`design-system-reuse`](./design-system-reuse/) | Reuse existing design-system primitives before creating UI. |
| [`legacy-mobile-style-compat`](./legacy-mobile-style-compat/) | Keep UI styles compatible with older mobile browsers and WebViews. |

## Install

Copy an individual skill directory to the location used by your coding agent.

### Codex

```text
.codex/skills/<skill-name>/
└── SKILL.md
```

Use `~/.codex/skills/<skill-name>/` for a user-level Codex installation.

### Claude Code

```text
.claude/skills/frontend-change-scout/
└── SKILL.md
```

Use `~/.claude/skills/frontend-change-scout/` for a user-level Claude Code installation. The skills in this repository use the portable Agent Skills format; Codex-specific `agents/openai.yaml` files are optional metadata and are not required by Claude Code.

## Use

Ask explicitly for the skill when you want predictable activation:

```text
Use $frontend-change-scout to map the smallest safe change for this frontend request. Do not edit code.
```

The skill reports the relevant route, components, state/data flow, styling, tests, constraints, and the smallest validation command. It performs reconnaissance only; implementation remains a separate request.
