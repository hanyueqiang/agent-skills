---
name: legacy-mobile-style-compat
description: Build, review, and repair CSS for older mobile browsers and embedded WebViews, including Android 8 and Android 9-era engines. Use when editing React, Vue, Angular, HTML/CSS, CSS Modules, Sass, Less, styled components, or other web UI styles that must work without modern CSS layout assumptions. Detect risky declarations, choose resilient fallbacks, and preserve the existing design without framework-specific rules.
---

# Legacy Mobile Style Compatibility

Support the actual browser engine, not just the operating-system label. Before changing layout CSS, identify the minimum Android/iOS browser or embedded WebView version from project requirements, analytics, device matrix, or user-provided target. If it is unknown, treat modern layout features as compatibility risks and state the assumption.

## Target triage (first response)

Before investigating or changing styles, do one of the following:

- If the user supplied a target, restate the minimum supported browser or WebView engine.
- If the target is missing, ask this concise question and continue reconnaissance with the conservative baseline below:

  ```text
  请确认最低兼容目标：Android 8/9 的系统浏览器或 Chrome、Android System WebView 最低版本，还是特定 App 内嵌 WebView？未提供时按 Android 8 + 未知旧 WebView 处理。
  ```

- Record `Android 8-era WebView (assumed)` whenever the user does not answer. Do not claim Android 8/9 support from a desktop screenshot alone.
- Treat Android 9 as a browser/WebView version to verify, not as an automatic ban on every modern layout feature. Android system version and embedded-engine version can differ.

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
| `inset` and all logical layout properties | `inset-inline`, `margin-inline`, `padding-block`, `inline-size`, and related properties may be unavailable. | Explicit physical offsets, margins, padding, `width`, and `height`. |
| `justify-content: start` / `end` | Legacy flexbox expects flex-specific values. | `flex-start` / `flex-end`. |
| CSS Grid as the only layout | Basic Grid can work in Android 9-era Chromium, but not necessarily in an older or custom embedded WebView. | Verify the engine; otherwise provide a Flexbox or block-layout fallback. |
| `position: sticky` as essential behavior | It can work in Android 9-era Chromium but commonly fails with nested scroll containers or ancestor `overflow`. | Keep the non-sticky layout usable and test the actual scroll container; add JavaScript only when sticky behavior is essential. |
| `dvh`, `svh`, `lvh` | Dynamic viewport units are modern. | `vh` baseline plus an existing project runtime viewport-variable pattern, if present. |
| `env(safe-area-inset-*)` without fallback | Older browsers do not expose safe-area variables. | A normal padding baseline, then `env()` as an enhancement. |
| `min()`, `max()`, `clamp()` | Android 8/9-era engines can predate these sizing functions. | Fixed baseline values and conventional media queries; use the function only as an enhancement. |
| `:is()`, `:where()`, `:has()`, `:focus-visible` | Unsupported selectors can cause a complete selector rule to be ignored. | Ordinary selectors and a `:focus` baseline; add modern selectors separately as enhancements. |
| `@container`, container units, native nesting, `@layer` | Modern at-rules and units are not available in Android 8/9-era engines. | Conventional selectors and viewport media queries; compile Sass/Less nesting before shipping. |
| `backdrop-filter`, standard `mask-*` | Visual effects are too new to be required for older WebViews. | Opaque/semitransparent color, a normal gradient, or an image fallback. |
| Standard `line-clamp` | The unprefixed form has limited support. | `display: -webkit-box`, `-webkit-box-orient: vertical`, `-webkit-line-clamp`, and `overflow: hidden`. |
| CSS Scroll Snap as essential interaction | Older engines may not provide reliable snapping or associated properties. | Make ordinary touch scrolling usable first; add JavaScript paging only when required. |

## Layout rewrites

- Replace horizontal flex gaps with `.item + .item { margin-left: <space>; }`.
- Replace vertical gaps with `.item + .item { margin-top: <space>; }`.
- For a wrapping row, prefer equal item margins and a compensating parent margin only when it matches the existing layout. Verify the first/last row.
- Replace `inset: 0` with all four explicit offsets.
- Do not add `@supports` as the only fallback for a layout that must work everywhere; ensure the baseline CSS is already usable.
- Do not add browser-sniffing or a polyfill for one declaration without evidence that the CSS fallback fails.
- For a fixed footer, bottom sheet, or input area, test keyboard opening, viewport resize, and nested scrolling. Do not assume `100vh` or `position: fixed` has the same visual viewport behavior in every WebView.
- Do not ban CSS custom properties, `calc()`, ordinary Flexbox, transforms, or `object-fit` solely because the target is Android 8/9; verify the actual engine before adding restrictions.

## Framework boundaries

- **React:** inspect CSS Modules, global CSS, CSS-in-JS, and component props that produce styles. Keep fallback styles with the component unless the project already centralizes them.
- **Vue:** inspect scoped and unscoped `<style>` blocks, preprocessor files, and dynamic class/style bindings. Preserve scope attributes and existing class names.
- **Any framework:** the browser receives CSS, so apply the same layout and fallback rules regardless of rendering framework.

## Review and verification

Search only the touched paths for common risks:

```bash
rg -n --glob '*.{css,scss,sass,less,vue,jsx,tsx,js,ts}' '\b(gap|row-gap|column-gap|aspect-ratio|inset|inset-inline|inset-block|margin-inline|margin-block|padding-inline|padding-block|inline-size|block-size|backdrop-filter|line-clamp|scroll-snap-[a-z-]+)\s*:|justify-content\s*:\s*(start|end)\b|\b(position\s*:\s*sticky|\d+(dvh|svh|lvh)|@container|@layer|:(has|is|where|focus-visible)\b|(min|max|clamp)\()' <paths>
```

Then validate the changed layout at the smallest supported viewport. When device testing is unavailable, use the project's browser target configuration and record the unverified risk; do not claim Android 8/9 support based only on a desktop browser screenshot.

## Required output

```md
## Legacy mobile compatibility check

**Target engine:** <known version or assumption>
**Target confirmation:** confirmed | `Android 8-era WebView (assumed)`
**Changed UI:** <component and behavior>

| Location | Risk | Fallback / decision | Evidence |
| --- | --- | --- | --- |
| `path:line` | flex gap | adjacent-sibling margin | <target or existing pattern> |

**Verification:** <device, emulator, browser target, or `not available`>
**Remaining risk:** <none or concrete limitation>
```

Keep compatibility fixes local, semantic, and minimal. Do not redesign the page or introduce a framework-specific abstraction solely to avoid one CSS declaration.
