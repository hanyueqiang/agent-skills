---
name: design-to-ui-converter
description: Convert a design draft, exported HTML/CSS, screenshot, or prototype into production UI in an existing frontend project. Use when implementing or reviewing design-to-code work for React, Vue, Angular, HTML/CSS, or another web UI stack, including requests that reference Lanhu/BlueLake, Figma exports, a visual draft, or exported DOM/CSS. Reuse the project's installed UI component library and local conventions; use semantic custom components only when no suitable project component exists.
---

# Design to UI Converter

Turn the design artifact into maintainable product UI, not a literal copy of exported markup. Preserve the visual hierarchy and interaction intent while following the target repository's framework, UI library, styling, data, asset, and test conventions.

## Inspect before implementation

1. Read repository instructions, the target page or feature, nearby components, and the design artifact before editing.
2. Identify the framework, language, styling approach, unit strategy, asset conventions, routing/entry point, and the smallest relevant test or lint command.
3. Check package manifests, lockfiles, application setup, and nearby usages to identify the installed UI component library. Confirm its actual APIs from existing call sites.
4. Map the design into product responsibilities: page layout, reusable controls, dynamic states, backend data, loading/empty/error states, and required interactions.

Do not assume Vue, React, TypeScript, rem scaling, or a component library from the design artifact.

## Choose controls deliberately

For every design control, choose the first option that fits:

1. Reuse a matching control from the installed UI component library, following existing project usage.
2. Compose existing local components when they cover the behavior and visual structure.
3. Create a focused semantic component only when neither exists; keep it local to the feature unless a second credible use already exists.
4. Use native semantic elements for simple controls when that is the established convention or the smallest correct solution.

Never add a UI library for one screen or replace an installed library with raw lookalike elements. If no UI library is installed, follow the nearest repeated local pattern and implement only the needed component behavior.

## Implement the design

- Replace exported class names and nested wrapper noise with meaningful, feature-oriented structure.
- Keep text, lists, status, and media data-driven. Use the real project data/API/store patterns; do not ship design sample copy or remote mock URLs.
- Reuse project tokens, icons, assets, typography, spacing, breakpoints, and unit conversion. Do not invent a new scale from design pixels.
- Preserve semantic HTML, keyboard behavior, labels, focus states, loading, empty, error, and disabled states.
- Use local assets or the project's icon system. Use CSS for simple decoration only when it is stable and accessible.
- Wire the feature into the existing route, import, or parent flow when the request requires it to be visible.
- For mobile or embedded-WebView targets, retain the project's proven compatibility and fallback patterns.

## Verify

1. Compare the implemented states against the artifact: hierarchy, spacing, type, color, borders, imagery, responsive behavior, and interactions.
2. Run the smallest relevant existing lint, type-check, test, or build command for touched files.
3. Report unavailable validation and any remaining design assumptions rather than claiming pixel accuracy without evidence.

## Output before editing

For a planning, review, or implementation request, state this compact record before changing code. Implement only when asked.

```md
## Design-to-UI plan

**Artifact:** <source and relevant states>
**Project conventions:** <framework, styles, units, data/assets>
**UI library:** <package and evidence, or `none found`>

| Design element | Implementation | Evidence |
| --- | --- | --- |
| <control/layout> | <library component, local component, or semantic custom component> | <path/API/call site> |

**Files to change:**
- `<path>` — <smallest reason>

**Validation:** `<command>`
**Assumptions / gaps:** <only unresolved design or behavior details>
```
