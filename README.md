# @metrifi/brand

MetriFi's **Instrument** design system: the single source of truth for design tokens, base
styles, components and brand assets across every MetriFi property, from metrifi.com to the
platform and devops.metrifi.com.

- **The rules:** [`DESIGN.md`](DESIGN.md): what Instrument is, and how marketing pages and product
  screens use it differently.
- **The spec:** [`css/tokens.css`](css/tokens.css): every token as a CSS variable.
- **The look:** open [`specimen.html`](specimen.html) in a browser for every token, both contexts.

Dark-only. One typeface (Inter). One place to change a color; every property follows.

---

## What's in the box

| Import | Contents |
|--------|----------|
| `@metrifi/brand` / `@metrifi/brand/index.css` | Everything: fonts + tokens + base + components |
| `@metrifi/brand/tokens.css` | Just the CSS variables (bring your own element styles) |
| `@metrifi/brand/app.css` | **shadcn/ui theme** — Instrument mapped onto shadcn's variable contract (for React + Tailwind + shadcn apps). Pulls in tokens. |
| `@metrifi/brand/fonts.css` | Inter (variable, 300 to 700) via Google Fonts |
| `@metrifi/brand/base.css` | Element resets, product type defaults, and the `.site-*` / `.ui-*` type recipe classes (needs tokens) |
| `@metrifi/brand/components.css` | Plain-CSS components: `.card`, `.btn`, inputs, `.scopes`… (plain-HTML/Blade surfaces) |
| `@metrifi/brand/logo.svg` | Wordmark + violet mark |
| `@metrifi/brand/favicon.svg` | Favicon |

---

## Install

The repo is **public**, so a plain GitHub install needs no auth (this is how the platform and the website consume it). Two ways — pick one.

### A. Straight from GitHub (no registry, works immediately)
```bash
npm install bloomcu/metrifi-brand          # latest main
npm install bloomcu/metrifi-brand#v0.2.0    # pin a tag
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

### Next.js (e.g. devops.metrifi.com)
```ts
// app/layout.tsx
import "@metrifi/brand/tokens.css";  // tokens only, with your own component CSS
// or: import "@metrifi/brand";      // the full plain-CSS system
```
Favicon: copy `@metrifi/brand/favicon.svg` into `public/`, or reference it via your asset pipeline.

### Vite (e.g. the Laravel platform)
```css
/* resources/css/app.css */
@import "@metrifi/brand";
```
```blade
{{-- in your Blade layout --}}
@vite('resources/css/app.css')
```

### Astro + Tailwind (metrifi.com)
The website is a shadcn/Tailwind consumer: import the shadcn theme and the font, never the full
bundle (its element styles would fight Tailwind). Build marketing type from the `--site-*` tokens
or the `.site-*` recipe classes (copy those few rules if you don't load `base.css`).
```css
/* src/styles/globals.css */
@import "@metrifi/brand/app.css";
@import "@metrifi/brand/fonts.css";
```

### Plain HTML
Copy `css/` and `assets/` into your static dir and link `index.css`:
```html
<link rel="stylesheet" href="/brand/css/index.css">
<link rel="icon" href="/brand/assets/favicon.svg">
```

---

## Theming a shadcn app

For a **React + Tailwind v4 + shadcn/ui** app (apps and the React islands on websites), use
`app.css` instead of the full bundle — it maps Instrument onto shadcn's variable contract so every
shadcn component is on-brand with no per-component work.

1. **Import the theme** in your Tailwind/CSS entry, and expose the vars to Tailwind utilities:
   ```css
   /* app/globals.css */
   @import "tailwindcss";
   @import "@metrifi/brand/app.css";   /* Instrument → shadcn vars (dark-only) */
   @import "@metrifi/brand/fonts.css";  /* optional: Inter */

   @theme inline {
     --color-background: var(--background);
     --color-foreground: var(--foreground);
     --color-card: var(--card);
     --color-card-foreground: var(--card-foreground);
     --color-popover: var(--popover);
     --color-popover-foreground: var(--popover-foreground);
     --color-primary: var(--primary);
     --color-primary-foreground: var(--primary-foreground);
     --color-secondary: var(--secondary);
     --color-secondary-foreground: var(--secondary-foreground);
     --color-muted: var(--muted);
     --color-muted-foreground: var(--muted-foreground);
     --color-accent: var(--accent);
     --color-accent-foreground: var(--accent-foreground);
     --color-destructive: var(--destructive);
     --color-destructive-foreground: var(--destructive-foreground);
     --color-border: var(--border);
     --color-input: var(--input);
     --color-ring: var(--ring);
     --radius-lg: var(--radius);
     --radius-md: calc(var(--radius) - 2px);
     --radius-sm: calc(var(--radius) - 4px);
   }
   ```
2. **Don't toggle a dark class** — MetriFi is dark-only; the theme is always on. Skip
   `next-themes` / the `.dark` switch.
3. **Keep the signature squared button.** `--radius` is set to the card value (12px) so cards,
   popovers, and inputs read right — but Instrument buttons are near-square (2px). In your copied
   `button.tsx`, set the radius to Instrument's `--radius-button`:
   ```diff
   - "... rounded-md ..."
   + "... rounded-[var(--radius-button)] ..."
   ```
4. **Don't** also import `base.css`/`components.css` here — those are for plain-HTML/Blade surfaces
   and clash with the shadcn `--muted` semantics (see `app.css` header).

The full token → shadcn mapping (and the why) lives in [`css/app.css`](css/app.css) and
[`DESIGN.md`](DESIGN.md).

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

**The one rule:** green is for data and the primary button; violet is for links, focus and the
interface. Heading emphasis is violet, and flips to green on a marketing card marked
`.mf-surface`. Full reasoning in [`DESIGN.md`](DESIGN.md#2-color).

---

## Releasing a change

Consumers pin a git tag, so a brand change reaches them only when each one bumps its
pin. That's intentional: styling can't shift under a deploy without a reviewed commit.
To ship a change:

1. **In this repo**: edit the CSS, assets or `DESIGN.md` on a branch, bump `version` in
   `package.json` (semver, see below), and open a PR. After it merges, tag the merge commit
   and push the tag:
   ```bash
   git tag v1.0.0 origin/main && git push origin v1.0.0
   ```
2. **Each property** bumps its pin in its own PR, checks its pages, and deploys the usual way:
   ```bash
   npm install bloomcu/metrifi-brand#v1.0.0   # updates package.json + lockfile
   ```
   | Property | Repo | Notes |
   |----------|------|-------|
   | platform.metrifi.com | `metrifi/metrifi-platform` | Blade pages import the full bundle in `resources/css/app.css`; the product header uses `product-shell.css` |
   | metrifi.com | `bloomcu/metrifi-website` | `app.css` + `fonts.css` in `src/styles/globals.css` |
   | devops.metrifi.com | `metrifi/metrifi-devops` | Reads the tokens; fonts are self-hosted |

## Moving a property to v1.0.0

DESIGN.md §11 lists what changed. Per property:

**The platform** (`metrifi/metrifi-platform`, from v0.5.0): bump the pin. Then, in its own CSS,
switch every `font-family: var(--font-mono)` on a label, eyebrow, tab, badge, nav link or number
to `var(--font-body)` (keep mono on code and copyable values), drop sizes and weights that
restate a recipe in favor of the recipe tokens, and put product screens in sentence case
except eyebrows and field labels.

**metrifi.com** (`bloomcu/metrifi-website`, from v0.4.0; v0.5.0 added `color-scheme: dark`):
bump the pin. Its `html.mf-site` block can drop the four `--font-*` overrides (the brand is Inter
now) and point its type tokens at the brand's: `--s-h1: var(--site-h1)` and so on, and
`--s-quiet` at `--text-tertiary` (#9890B0, a shade lighter than its #8E86A8). Its components
stay in the website; they are marketing-only and already follow DESIGN.md §5.

**devops.metrifi.com** (`metrifi/metrifi-devops`, no dependency yet): add the package and import
`tokens.css` only. Keep self-hosting Inter, and point the brand's font names at it:
`--font-body: var(--font-inter), "Inter", system-ui, sans-serif` (same for `--font-display`).
Then replace the copied dark tokens with references:

| devops token | brand token |
|--------------|-------------|
| `--bg-canvas`, `--bg-panel`, `--bg-raised` | `--canvas`, `--surface`, `--elevated` |
| `--bg-hover`, `--bg-selected`, `--scrim` | `--ui-hover`, `--ui-selected`, `--ui-scrim` |
| `--text-primary` ... `--text-quaternary` | same names |
| `--line-1`, `--line-2`, `--line-3` | `--line-faint`, `--line` (.08; devops used .09), `--line-strong` |
| `--control-edge` (.40) | `--control-edge` (.35; devops chose .40 for margin on raised panels) |
| `--violet`, `--green`, `--data-ink` | `--violet`, `--green`, `--green` |
| `--font-ui`, `--font-display` | `--font-body` |

Its light theme (`[data-theme="light"]`) has no brand counterpart: the brand is dark-only. Until
that changes, devops keeps its light values as a local exception layered over the brand tokens.

## Versioning

Semver. Token value changes or renames are breaking (major). Additive tokens/components are minor.
Docs-only or asset tweaks are patch. Tag releases (`v1.0.0`) so git-based installs can pin.
v1.0.0 is the first major: Inter replaced Space Grotesk and monospace labels (DESIGN.md §11).
