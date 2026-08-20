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
| `@metrifi/brand/app.css` | **shadcn/ui theme** — Instrument mapped onto shadcn's variable contract (for React + Tailwind + shadcn apps). Pulls in tokens. |
| `@metrifi/brand/fonts.css` | Space Grotesk + Inter via Google Fonts |
| `@metrifi/brand/base.css` | Element resets + `body`/headings/utilities (needs tokens) |
| `@metrifi/brand/components.css` | Plain-CSS components: `.card`, `.btn`, inputs, `.scopes`… (plain-HTML/Blade surfaces) |
| `@metrifi/brand/logo.svg` | Wordmark + violet mark |
| `@metrifi/brand/favicon.svg` | Favicon |

---

## Install

The repo is **public**, so a plain GitHub install needs no auth (this is how the gateway and IdP consume it). Two ways — pick one.

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

## Theming a shadcn app

For a **React + Tailwind v4 + shadcn/ui** app (apps and the React islands on websites), use
`app.css` instead of the full bundle — it maps Instrument onto shadcn's variable contract so every
shadcn component is on-brand with no per-component work.

1. **Import the theme** in your Tailwind/CSS entry, and expose the vars to Tailwind utilities:
   ```css
   /* app/globals.css */
   @import "tailwindcss";
   @import "@metrifi/brand/app.css";   /* Instrument → shadcn vars (dark-only) */
   @import "@metrifi/brand/fonts.css";  /* optional: Space Grotesk + Inter */

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

**The one rule:** violet is a *canvas* accent, green is a *surface* accent. Put `.mf-surface` on
any card so emphasis flips to green inside it. Full reasoning in [`DESIGN.md`](DESIGN.md#2-color).

---

## Releasing a change

Consumers pin a git tag, so a brand change reaches them only when each one bumps its
pin. That's intentional — styling can't shift under a deploy without a reviewed commit.

### 1. Cut the release here

Edit the CSS / assets / `DESIGN.md`, bump `version` in `package.json` (semver, see below),
commit, then tag and push. There is no CI in this repo, so check by hand that every
`var(--token)` you touched still resolves before tagging.

```bash
git commit -am "…"                          # the change
git tag v0.4.0 && git push origin main v0.4.0
```

### 2. Find who actually consumes it

**Don't trust a list in this file — it rots.** A hardcoded list here went stale once already,
pointing at two retired repos for months. Ask GitHub instead:

```bash
for org in bloomcu metrifi; do
  for r in $(gh repo list $org --limit 100 --json name --jq ".[].name"); do
    gh api "repos/$org/$r/contents/package.json" --jq .content 2>/dev/null | base64 -d 2>/dev/null \
      | grep -o "\"@metrifi/brand\": \"[^\"]*\"" | sed "s|^|  $org/$r -> |"
  done
done
```

As of v0.4.0 that returns five live MetriFi-owned surfaces — `metrifi/metrifi-platform`
(the monolith, Forge), and `bloomcu/ai-metrifi-com`, `bloomcu/launch`, `bloomcu/reports`,
`bloomcu/metrifi-website` (all Vercel) — plus dormant hits in `metrifi-id`,
`metrifi-mcp-gateway`, `metrifi-mcp-gateway-laravel`, `metrifi-site-builder-laravel` and
`ai-metrifi-com-legacy`, all of which are retired. **Skip the retired ones.**

**No client site may appear in that list.** Client sites must never consume `@metrifi/brand`
(brand bleed) — they get their own tokens from the Site Builder. If a client repo ever shows
up there, that is the bug, not the pin.

### 3. Bump each live consumer

```bash
npm install bloomcu/metrifi-brand#v0.4.0    # updates package.json + lockfile
npm run build                               # confirm it still builds
git commit -am "Bump @metrifi/brand to v0.4.0"
```

Open a PR rather than pushing — `metrifi-platform`'s `main` is push-to-deploy on Forge, and
the Vercel sites deploy from their default branch too.


## Versioning

Semver. Token value changes or renames are breaking (major). Additive tokens/components are minor.
Docs-only or asset tweaks are patch. Tag releases (`v0.1.0`) so git-based installs can pin.
