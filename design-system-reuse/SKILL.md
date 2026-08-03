---
name: design-system-reuse
description: Reuse and extend an existing frontend design system before creating UI components. Use when implementing, planning, reviewing, or refactoring React, Vue, Angular, HTML/CSS, or other web UI in a team codebase that may already contain components, design tokens, icons, patterns, Storybook stories, or style conventions. Helps choose reuse, composition, extension, or a justified new primitive.
---

# Design System Reuse

Treat the existing design system as the default solution. Before creating UI, find the closest established primitive and its real usage. Do not introduce a parallel component, token, icon, or styling pattern when an existing one satisfies the requirement.

## Discovery

1. Identify the requested UI behavior, states, and page context.
2. Inspect repository instructions, package scripts, and the nearest feature before searching broadly.
3. Search for relevant components, tokens, icons, hooks, styles, stories, and tests. Prefer exact concepts from the request and visible UI labels.
4. Read the candidate's public API and two or three real call sites. Inspect its styles and tests only when they affect the requested state or variant.
5. Record project conventions: component location, export style, styling method, token source, icon source, test pattern, and accessibility conventions.

## Make a reuse decision

Classify each requested UI element before implementation:

| Decision | Use when | Action |
| --- | --- | --- |
| **Reuse** | An existing primitive supports the behavior and visual treatment. | Use it unchanged. |
| **Compose** | Existing primitives can form the UI without changing their APIs. | Build the feature from those primitives. |
| **Extend** | One broadly useful, compatible addition serves at least two credible uses. | Add the smallest variant, slot, or state and update its tests/story. |
| **Create** | No candidate fits without misleading names, broken semantics, or duplicated styling. | Create one focused primitive following local conventions. |

Choose the highest row that works. Do not add a prop, variant, abstraction, or generic wrapper for one known call site. If a request is ambiguous, preserve the existing API and report the assumption.

## Guardrails

- Reuse semantic HTML and the project's accessibility behavior; do not replace a button, input, label, dialog, or focus treatment with a visually similar custom element.
- Use existing tokens rather than hard-coded color, spacing, typography, radius, shadow, breakpoint, or z-index values.
- Use the established icon package or local icon set. Do not add an icon dependency for one icon.
- Keep feature-specific layout and copy in the feature. Promote only genuinely shared behavior into the design system.
- Match the local styling and state-management pattern. Do not mix CSS approaches or introduce a second component library.
- When no design system exists, say so and follow the nearest repeated UI pattern; do not invent a broad system during a feature task.

## Output before changing code

For planning, review, or an implementation request, first provide this compact decision record. Then implement only if the user asked for implementation.

```md
## Design-system reuse plan

**Requested UI:** <behavior and states>
**Existing convention:** <component/style/token/icon/test pattern, or `none found`>

| UI element | Candidate | Decision | Evidence |
| --- | --- | --- | --- |
| <element> | `path/to/component` | reuse | <API or call site> |

**Files to change:**
- `path/to/file` — <smallest reason>

**Do not create:**
- <duplicate component, token, or dependency avoided>

**Validation:** `<smallest relevant command>`
**Assumptions / gaps:** <only scope-changing unknowns>
```

Do not claim a design-system primitive exists without a repository path and evidence.
