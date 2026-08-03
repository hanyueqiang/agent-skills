---
name: legacy-mobile-style-compat
description: Build, review, and repair CSS for older mobile browsers and embedded WebViews, including Android 8 and Android 9-era engines. Use when editing React, Vue, Angular, HTML/CSS, CSS Modules, Sass, Less, styled components, or other web UI styles that must work without modern CSS layout assumptions. Detect risky declarations, choose resilient fallbacks, and preserve the existing design without framework-specific rules.
---

# Legacy Mobile Style Compatibility

Support the actual browser engine, not just the operating-system label. Before changing layout CSS, identify the minimum Android/iOS browser or embedded WebView version from project requirements, analytics, device matrix, or user-provided target. If it is unknown, treat modern layout features as compatibility risks and state the assumption.

## Before styling

1. Inspect the changed component and its stylesheet, including Vue `<style>` blocks, CSS-in-JS templates, and imported CSS/Sass/Less files.
2. Find the nearest existing mobile fallback, reset, utility, or compatibility rule. Reuse that pattern.
3. Check only the declarations being added or modified. Do not perform a whole-repository rewrite during a feature task.
4. Prefer simple CSS that works in the target engine. Use JavaScript feature detection only when CSS fallback cannot preserve the required behavior.

## High-risk declarations and replacements

| Avoid relying on | Why | Prefer |
| --- | --- | --- |
| Flexbox `gap`, `row-gap`, `column-gap` | Older Android Chromium/WebViews do not support flex gaps. | Adjacent-sibling margins; for wrapped rows, item margins with limited parent compensation. |
| `aspect-ratio` | Unsupported in older engines. | Explicit dimensions, or a padding-based ratio box when a fixed ratio is required. |
| `inset` and logical inset properties | Modern shorthand/logical positioning may be unavailable. | Explicit `top`, `right`, `bottom`, `left`. |
| `justify-content: start` / `end` | Legacy flexbox expects flex-specific values. | `flex-start` / `flex-end`. |
| CSS Grid as the only layout | Older embedded engines may not support the required Grid behavior. | Flexbox or block layout fallback; use Grid only after the target matrix confirms it. |
| `position: sticky` as essential behavior | Sticky support varies in older WebViews and nested scroll containers. | A non-sticky usable layout; add a tested JavaScript fallback only when sticky behavior is required. |
| `dvh`, `svh`, `lvh` | Dynamic viewport units are modern. | `vh` baseline plus an existing project runtime viewport-variable pattern, if present. |
| `env(safe-area-inset-*)` without fallback | Older browsers do not expose safe-area variables. | A normal padding baseline, then `env()` as an enhancement. |

## Layout rewrites

- Replace horizontal flex gaps with `.item + .item { margin-left: <space>; }`.
- Replace vertical gaps with `.item + .item { margin-top: <space>; }`.
- For a wrapping row, prefer equal item margins and a compensating parent margin only when it matches the existing layout. Verify the first/last row.
- Replace `inset: 0` with all four explicit offsets.
- Do not add `@supports` as the only fallback for a layout that must work everywhere; ensure the baseline CSS is already usable.
- Do not add browser-sniffing or a polyfill for one declaration without evidence that the CSS fallback fails.

## Framework boundaries

- **React:** inspect CSS Modules, global CSS, CSS-in-JS, and component props that produce styles. Keep fallback styles with the component unless the project already centralizes them.
- **Vue:** inspect scoped and unscoped `<style>` blocks, preprocessor files, and dynamic class/style bindings. Preserve scope attributes and existing class names.
- **Any framework:** the browser receives CSS, so apply the same layout and fallback rules regardless of rendering framework.

## Review and verification

Search only the touched paths for common risks:

```bash
rg -n --glob '*.{css,scss,sass,less,vue,jsx,tsx,js,ts}' '\b(gap|row-gap|column-gap|aspect-ratio|inset|inset-inline|inset-block)\s*:|justify-content\s*:\s*(start|end)\b|\b(position\s*:\s*sticky|\d+(dvh|svh|lvh))\b' <paths>
```

Then validate the changed layout at the smallest supported viewport. When device testing is unavailable, use the project's browser target configuration and record the unverified risk; do not claim Android 8/9 support based only on a desktop browser screenshot.

## Required output

```md
## Legacy mobile compatibility check

**Target engine:** <known version or assumption>
**Changed UI:** <component and behavior>

| Location | Risk | Fallback / decision | Evidence |
| --- | --- | --- | --- |
| `path:line` | flex gap | adjacent-sibling margin | <target or existing pattern> |

**Verification:** <device, emulator, browser target, or `not available`>
**Remaining risk:** <none or concrete limitation>
```

Keep compatibility fixes local, semantic, and minimal. Do not redesign the page or introduce a framework-specific abstraction solely to avoid one CSS declaration.
