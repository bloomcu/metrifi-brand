# @metrifi/brand

MetriFi's **Instrument** design system — the single shared source of truth for design tokens,
base styles, components, and brand assets across every MetriFi app and website.

- **The rules:** [`DESIGN.md`](DESIGN.md) — what Instrument is and how to use it.
- **The spec:** [`css/tokens.css`](css/tokens.css) — every token as a CSS variable.

Dark-only, product-focused. One place to change a color; every surface follows.

---

## What's in the box

| Import | Contents |
|--------|----------|
| `@metrifi/brand` / `@metrifi/brand/index.css` | Everything: fonts + tokens + base + components |
| `@metrifi/brand/tokens.css` | Just the CSS variables (bring your own element styles) |
| `@metrifi/brand/fonts.css` | Space Grotesk + Inter via Google Fonts |
| `@metrifi/brand/base.css` | Element resets + `body`/headings/utilities (needs tokens) |
| `@metrifi/brand/components.css` | Plain-CSS components: `.card`, `.btn`, inputs, `.scopes`… |
| `@metrifi/brand/logo.svg` | Wordmark + violet mark |
| `@metrifi/brand/favicon.svg` | Favicon |

---

## Install

It's a private package. Two ways to consume it — pick one.

### A. Straight from GitHub (no registry, works immediately)
```bash
npm install bloomcu/metrifi-brand          # latest main
npm install bloomcu/metrifi-brand#v0.1.0    # pin a tag
```

### B. GitHub Packages registry (versioned)
Add an `.npmrc` so the `@metrifi` scope resolves to GitHub Packages, then install:
```
# .npmrc
@metrifi:registry=https://npm.pkg.github.com
```
```bash
npm install @metrifi/brand
```
Publishing is done from this repo with `npm publish` (CI or a maintainer with a
`write:packages` token). Bump the version in `package.json` first.

---

## Use it

### Next.js (e.g. the MCP gateway)
```ts
// app/layout.tsx
import "@metrifi/brand";          // full system
// or: import "@metrifi/brand/tokens.css";  // tokens only
```
Favicon: copy `@metrifi/brand/favicon.svg` into `public/`, or reference it via your asset pipeline.

### Vite (e.g. the Laravel IdP — stop hand-linking a static file)
```css
/* resources/css/app.css */
@import "@metrifi/brand";
```
```blade
{{-- in your Blade layout --}}
@vite('resources/css/app.css')
```

### Astro (websites)
```astro
---
import "@metrifi/brand";
---
```

### Plain HTML
Copy `css/` and `assets/` into your static dir and link `index.css`:
```html
<link rel="stylesheet" href="/brand/css/index.css">
<link rel="icon" href="/brand/assets/favicon.svg">
```

---

## Token cheat-sheet

```css
.thing {
  background: var(--surface);
  color: var(--text);
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
}
/* Primary CTA — green fill, hover flips to violet */
.cta { background: var(--cta); color: var(--cta-text); border-radius: var(--radius-button); }
.cta:hover { background: var(--cta-hover); }
```

**The one rule:** violet is a *canvas* accent, green is a *surface* accent. Put `.mf-surface` on
any card so emphasis flips to green inside it. Full reasoning in [`DESIGN.md`](DESIGN.md#2-color).

---

## Versioning

Semver. Token value changes or renames are breaking (major). Additive tokens/components are minor.
Docs-only or asset tweaks are patch. Tag releases (`v0.1.0`) so git-based installs can pin.
