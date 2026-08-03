---
name: frontend-change-scout
description: Map the smallest safe change before modifying a frontend codebase. Use when planning a React, Vue, Next.js, Nuxt, Angular, or web UI feature, bug fix, or refactor and the user needs the relevant route, components, state/data flow, styles, tests, constraints, and minimal validation command identified first. Do not use for implementation, whole-repository documentation, or broad architecture onboarding.
---

# Frontend Change Scout

Produce evidence-backed reconnaissance only. Do not edit files, install packages, run a broad test suite, or propose an implementation until the user asks.

## Workflow

1. Restate the requested behavior in one sentence. Record missing details as assumptions; do not block on low-risk ambiguity.
2. Find the closest entry point: route, page, feature folder, Storybook story, or visible user-facing text. Prefer project-provided code graph tools; otherwise use narrow symbol and filename searches.
3. Read only the entry point and its direct dependencies. Follow this chain only while it is relevant:

   `route/page → component → hook/store → data/API → style/design token → test`

4. Stop expanding when every requested behavior has an owner and there is evidence for the affected boundary. Do not read sibling features, generated files, lockfiles, or unrelated shared utilities.
5. Inspect nearby tests and project scripts only when they affect the requested behavior or determine the smallest verification command.
6. Report the map using the required format. State `unknown` instead of guessing.

## Frontend-specific checks

Check only applicable categories:

- **Routing:** URL, route configuration, layouts, guards, query or path parameters.
- **Components:** page owner, component hierarchy, shared component reuse, props/events.
- **State and data:** local state, context/store, query cache, API client, loading/error/empty states.
- **Styling:** CSS module, Tailwind, CSS-in-JS, design tokens, responsive and dark-mode behavior.
- **Tests:** nearest unit/component/E2E test, mocks/fixtures, and visual-regression coverage.
- **Constraints:** accessibility, localization, feature flags, permissions, analytics, performance budgets, and repository instructions.

## Scope rules

- Prefer existing components, hooks, tokens, and test patterns; name the evidence that supports reuse.
- Classify each file as **required**, **likely**, or **not needed**. A file is required only when it owns requested behavior or a direct contract.
- Limit the first pass to the entry point plus one dependency hop. Expand one hop at a time only to resolve a named unknown.
- Treat generated outputs and third-party code as out of scope unless the task explicitly targets them.
- For a cross-cutting request, split the map by user flow rather than listing the entire repository.

## Required output

```md
## Minimal change map

**Requested behavior:** <one sentence>
**Confidence:** high | medium | low

### Required files
| File | Owner / relevance | Evidence |
|---|---|---|
| `path/to/file` | <what it owns> | <symbol, import, route, or test> |

### Likely files
| File | Why it may change | What would confirm it |
|---|---|---|

### Not needed
- `path/or/area` — <why it is outside the behavior>

### Dependency path
`route/page → component → state/data → style → test`

### Constraints and risks
- <constraint or `None found`>

### Smallest validation
`<exact command>`

### Unknowns / assumptions
- <only unresolved facts that could change scope>
```

Use file paths and symbols discovered from the repository. Keep the result compact: name files, owners, evidence, and next checks; do not summarize unrelated source code.
