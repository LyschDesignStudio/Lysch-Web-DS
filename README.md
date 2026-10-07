# Lysch Web DS

Design tokens of the Lysch Web DS as CSS custom properties, generated from the Figma foundation (`foundation/*.json`).

## Install

The repository is private, so installing needs a GitHub account with access to LyschDesignStudio.

```bash
npm install github:LyschDesignStudio/Lysch-Web-DS
```

To pin a version, add a tag: `github:LyschDesignStudio/Lysch-Web-DS#v0.1.0`.

## Usage

With a bundler (Vite, Next.js, Astro…), import it once at the app's entry point:

```js
import '@lyschdesignstudio/web-ds';
```

From CSS:

```css
@import '@lyschdesignstudio/web-ds';
```

Without a bundler, link the file from `node_modules` or copy it:

```html
<link rel="stylesheet" href="node_modules/@lyschdesignstudio/web-ds/css/lysch-web-ds.css">
```

Then use the tokens:

```css
.card {
  background: var(--ds-color-surface-secondary);
  color: var(--ds-color-content-primary);
  padding: var(--ds-space-inset-lg);
  border-radius: var(--ds-radius-container-sm);
}
```

```html
<h1 class="ds-type-display-lg">Title</h1>
```

| Import | What it has |
| --- | --- |
| `@lyschdesignstudio/web-ds` | Everything: primitives, theme (light + dark), modes and typography |
| `@lyschdesignstudio/web-ds/tokens/<layer>.css` | One layer only: `primitives`, `theme`, `density`, `platform`, `motion`, `viewport` |
| `@lyschdesignstudio/web-ds/typography.css` | One class per text style: `.ds-type-display-lg`, `.ds-type-body-md`, … |
| `@lyschdesignstudio/web-ds/json/<file>.json` | The Figma token exports |


Every token is `--ds-` + the Figma path joined with `-`: `color.surface.primary` → `var(--ds-color-surface-primary)`. Primitives use `--ds-ref-…`; use them only inside the DS, and use the semantic tokens on sites.

## Modes

Each mode follows the system preference by default and can be forced with an attribute on `<html>` or on any element:

| Mode | Default | Force with |
| --- | --- | --- |
| Light / Dark | `prefers-color-scheme` | `data-mode="light"` / `"dark"` |
| Density | Regular | `data-density="compact"` / `"comfortable"` |
| Platform (minimum hit target) | `pointer: coarse` → Touch | `data-platform="web"` / `"touch"` |
| Motion | `prefers-reduced-motion` → Reduced | `data-motion="full"` / `"reduced"` |
| Viewport (grid, margins, display/headline/title) | `min-width` 768 / 1024 / 1440 | — |

An element with `data-mode` gets the colors of that mode, but properties it inherits (such as `color`) are still the parent's, so set `background` and `color` on it.

## Unit conversion

Figma exports numbers without units. The conversion is:

- space, size, radius, border, font size, line height, breakpoints, containers → `px`
- duration → `ms`
- opacity 0–100 → 0–1
- letter spacing (a % in Figma) → `em` (`-1` → `-0.01em`)
- font weight, layer (z-index), grid columns → no unit
- font families get a generic fallback (`sans-serif`, `serif`, `monospace`)

## Updating the tokens

1. Export the variables from Figma and replace the files in `foundation/`.
2. Run `npm run build` to regenerate `css/`. Never edit the CSS by hand.
3. Raise `version` in `package.json`, commit, and tag it (`git tag v0.2.0 && git push --tags`).
4. Projects update with `npm install github:LyschDesignStudio/Lysch-Web-DS#v0.2.0`.
