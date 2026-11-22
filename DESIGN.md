# MetriFi "Instrument" Design System

The visual language of every MetriFi property: the website (metrifi.com and campaign pages),
the platform (platform.metrifi.com), devops.metrifi.com, and anything new. Named **Instrument**:
precise, technical and quietly confident, like a well-made measuring tool. This document is the
written spec; [`css/tokens.css`](css/tokens.css) is the machine-readable one. When the two
disagree, the tokens win and this doc is wrong, so fix it.

> **Scope.** Dark mode is the only mode, on every property. One brand, two contexts:
> **marketing** pages persuade and explain, **product** screens get a task done. They share
> the palette, the typeface, the buttons and the copy rules (§1 to §4 and §9). They differ in
> type scale, density, cards and motion (§5 and §6). Pick the context per block, not per
> property: a public page on the platform that sells (the /connect hub) sets its introduction in
> marketing type, while its install steps, forms and copy controls use product type.

**v1.0.0 (November 2026) changed the type.** Inter is now the only typeface, for headings, body,
labels and numbers. Space Grotesk is retired, and monospace is for code and copyable values only.
This is the look metrifi.com converged on in October 2026 and devops.metrifi.com already uses.
§11 lists what changed for anyone moving up from 0.x.

---

## Which context a page uses

| | Marketing | Product |
|-|-----------|---------|
| **Where** | metrifi.com, campaign and landing pages, decks, the introduction of a public page that sells (the /connect hub) | Sign-in and sign-up, everything behind sign-in, settings, admin, dashboards, devops, and the steps, forms and copy controls on any page |
| **Job** | Persuade and explain, one idea per band | Get a task done, many facts per screen |
| **Headings** | Inter 400, big, tight tracking (`--site-*`) | Inter 500, or 600 at 20px and under, modest tracking (`--ui-*`) |
| **Body** | 17px at 400 | 14px at 400, 13px in rows, menus and buttons |
| **Layout** | The 1240px rail frame, bands of 88 to 160px | A shell with a header; forms, tables and panels, at most 1240px wide |
| **Cards** | Tiles with a 14px radius that glow on hover; one lit plate per page | Panels with a 12px radius, a hairline and an inset highlight; no glow |
| **Emphasis** | Color: a violet second clause in a heading | Weight, not color; green stays for data |
| **Motion** | Reveals on scroll, one eyebrow cursor that blinks three times | State changes only, 120 to 180ms |

The **primary button is the same everywhere**: green, 2px corners, flips to violet on hover.

---

## 1. Voice

Instrument reads like an instrument panel, not a brochure:

- **One typeface, set with care.** Inter everywhere. Big marketing headings are weight 400 with
  tight tracking; product titles are weight 500. Never bold for decoration.
- **Short labels in uppercase, letter-spaced.** Eyebrows above headings and form field labels:
  `FOR CREDIT UNIONS & BANKS`, `EMAIL`, `MCP CONNECTOR`. Everything else is sentence case (§3).
- **Copy as exact as the type.** Sentences end with punctuation, short copy has none, and units
  are written one way everywhere (§9).
- **Restraint with color.** The canvas is near-black violet; accents are used like punctuation,
  not paint. One accent per context (§2).

---

## 2. Color

### Surfaces (dark-only)
| Token | Hex | Use |
|-------|-----|-----|
| `--canvas` | `#0F0A1A` | Page background |
| `--surface` | `#1A1230` | Cards, panels |
| `--elevated` | `#241845` | Raised surfaces, menus, hovers |

### Accents: two colors, two jobs
- **Green is the data and measurement accent** (`--green #7DD4AA`): numbers, metrics, proof
  points, and the **primary button**. Green stands for the measured value, which is the whole
  "Instrument" idea, so it is at home on any background.
- **Violet is the text and interface accent** (`--violet #A074FF`): links, the active tab, the
  focus ring, selection, checkboxes, and heading emphasis.

**Heading emphasis is violet in both contexts:** the violet second clause (`h1 em`) is a brand
signature. On a marketing surface or card it flips to green (`.mf-surface h2 em`). **Inside
product prose, tables and forms, emphasis is weight, not color** (`base.css` sets `b` and
`strong` to 600 against the 400 body):
almost everything there sits on a panel, and green must stay free for data.

**Links are violet** on every surface (`.link`, `.foot a`, and links in running text), with
`--violet-hover` on hover. Green is never a link color.

| Token | Hex | |
|-------|-----|-|
| `--violet` / `--violet-hover` | `#A074FF` / `#B088FF` | Text and interface accent, focus ring |
| `--green` / `--green-hover` | `#7DD4AA` / `#93E0BB` | Data accent on any background, **primary button fill** |

### Status
| Token | Hex | Use |
|-------|-----|-----|
| `--gold` | `#D4B84A` | Warnings, pending |
| `--red` | `#E85D56` | Errors, destructive |

A number that went down is not an error: show it in a neutral color. Red is for a real problem.

### Text
Four steps, named the way devops.metrifi.com already names them.

| Token | Hex | Use | Contrast (canvas / surface / elevated) |
|-------|-----|-----|-----|
| `--text-primary` | `#E8E4F0` | Headings, values, body on product screens | AAA |
| `--text-secondary` | `#BFB8D3` | Body on marketing pages, descriptions, labels | 10.21 / 9.39 / 8.53 |
| `--text-tertiary` | `#9890B0` | Captions, column headers, timestamps, placeholders | 6.44 / 5.93 / 5.38 |
| `--text-quaternary` | `#736C8D` | Disabled text only (WCAG exempts it) | 3.95 / 3.63 / 3.30 |
| `--dark-text` | `#0F0A1A` | Text on green or violet fills | |

`--text` and `--muted` are the 0.x names and stay as aliases of the first two. New rules use the
`--text-*` names: shadcn's `app.css` reuses `--muted` for a surface color, so `--muted` means
different things in different consumers. Placeholders are text and need 4.5:1, so they take
`--text-tertiary`, never `--text-quaternary`.

### Hairlines and washes
Borders are **white at low alpha**, never solid grays: `--line-faint` (.06), `--line` (.08),
`--line-soft` (.10), `--line-strong` (.14). Tinted backgrounds use the wash tokens
(`--violet-08/12/20`, `--green-08/12/20`); selection is `--violet-20`.

**Anything you can operate takes `--control-edge` (.35) instead.** WCAG 2.1 SC 1.4.11 wants 3:1
between a control's visual boundary and its background; the hairline ramp tops out at 1.44:1 on
`--canvas`, while `--control-edge` measures 3.16:1. Inputs, selects, checkboxes, switches and
slider tracks are controls; a ruled grid is not.

---

## 3. Typography

**One family: Inter** (`--font-display` and `--font-body` are the same stack; both names stay so
older rules keep working). `fonts.css` loads it as one variable font, so any weight from 300 to
700 works. **`--font-mono` is for code, URLs, keys and IDs only**: an MCP URL, a prompt to paste,
a tool name like `sitebuilder__*`, a one-time code. Never for labels, eyebrows or numbers.

### Labels and eyebrows
- **Eyebrow** (above a heading): `.eyebrow`, 12px, weight 400, uppercase, `0.14em` tracking,
  `--text-secondary`. Violet when it leads a section on the canvas. Marketing may add the green cursor
  (§5). A `// ` prefix marks a section label on product screens and guides.
- **Field label** (names an input or a value): 11px, weight 500, uppercase, `0.1em` tracking,
  `--text-secondary` (`--label-*` tokens). `components.css` styles every `<label>` this way, and
  `.field-label` gives the same look to anything else that names a field. A `<label>` that wraps
  a checkbox or radio is a sentence ("Keep me signed in for 30 days"), so it stays sentence case
  at 14px.
- **Everything else is sentence case:** navigation, tabs, buttons, links, table column headers,
  badges and chips, menu items. A screen of uppercase headers reads as noise.

### Type recipes
Each role below is a set of tokens (size, line height, weight, tracking) and a class in
`base.css` that applies all of them: `.site-h1`, `.site-h2`, `.site-h3`, `.site-h4`,
`.site-quote`, `.site-lede`, `.site-body`, `.site-small` for marketing, and `.ui-display`,
`.ui-title`, `.ui-title-sm`, `.ui-lead`, `.ui-prose`, `.ui-text`, `.ui-caption` for product.
Use the class, or the tokens in your own rule. **Don't restate a size, weight or tracking a
recipe already has:** a property that sets its own drifts away from the brand the next time it
changes. `base.css` makes the product recipes the element defaults (body 14px, `h1` a page
title, `h2` a section title), because the plain-HTML entry serves product screens.

### Marketing scale (`--site-*`)
| Role | Size | Weight | Line height | Tracking |
|------|------|--------|-------------|----------|
| `h1` | `--site-h1` clamp(2.4rem, 5.3vw, 4.6rem) | 400 | 1.06 | -0.055em |
| `h2` | `--site-h2` clamp(2rem, 3.3vw, 2.9rem) | 400 | 1.07 | -0.045em |
| `h3` | `--site-h3` 1.375rem | 400 | 1.2 | -0.02em |
| `h4` | `--site-h4` 1.125rem | 400 | 1.3 | -0.015em |
| Pull quote | `--site-quote` clamp(1.875rem, 3.6vw, 3rem) | 400 | 1.12 | -0.035em |
| Lede | `--site-lede` 1.1875rem | 400 | 1.6 | |
| Body | `--site-body` 1.0625rem | 400 | 1.65 | |
| Small | `--site-small` 0.975rem | 400 | 1.55 | |

The second clause of a heading can be violet (`<em>` or a class); in an `h1` it takes its own line.
Big numbers are weight 400, green, `-0.045em`, tabular.

### Product scale (`--ui-*`)
| Role | Size / line height | Weight | Tracking |
|------|--------------------|--------|----------|
| Display: hero metric, a welcome screen's title | `--ui-display` clamp(2rem, 4vw, 2.75rem) / 1.08 | 500 | -0.03em |
| Page title | `--ui-title` 28 / 34 | 500 | -0.022em |
| Section title | `--ui-title-sm` 20 / 28 | 600 | -0.012em |
| Card title, lead, row name | `--ui-text-lg` 16 / 24 | 500 | |
| Prose, form fields, modal body | `--ui-text-md` 14 / 22 | 400 | |
| Rows, menus, buttons, tooltips | `--ui-text` 13 / 20 | 400 (450 for dense rows) | |
| Column headers, badges, chips, captions | `--ui-text-sm` 12 / 16 | 500 | |
| Tiny meta (a count in a tab, a footnote mark) | `--ui-text-xs` 11 / 16 | 500 | |

**Every number gets `font-variant-numeric: tabular-nums`** so columns line up. Line heights
are unitless (the pixel pairs above are what they come to), so a child at another size keeps a
proportional line box.

---

## 4. Shared components

### Buttons
- **Near-square corners**, `--radius-button: 2px`, on the primary button in both contexts. This
  squared-off button is a brand signature; don't round it.
- **Primary**: green fill, canvas-colored text, **hover flips to violet** (`--cta`, `--cta-hover`).
  Weight 500, sentence case.
- **Secondary**: marketing uses an outline (1px inset green or violet, 2px corners). Product uses
  a quiet button: `--elevated` or transparent fill, `--control-edge` border, `--ui-radius-control`
  (6px), 13px weight 500.
- `:active` nudges down 1px. Transitions use `--ease-standard` at about 140ms.

### Inputs
`--canvas` background (or `--site-field` on a marketing band), **`--control-edge` border**,
`--radius-sm` (6px), 14px text, placeholders in `--text-tertiary`. Focus is a violet border plus
a 3px `--violet-20` ring, so it shows at a glance. The label sits above the input, uppercase (§3).

### Copyable values
Anything someone reads in full or copies, such as MCP URLs, API tokens, keys and IDs, stays **fully
visible**. Let the value **wrap**; never hide characters behind a horizontal scrollbar, an
ellipsis or `text-overflow`. When a copy button shares the row, give the value its own full-width
line (label and button above, value below). Mono, `--text`, `overflow-wrap: anywhere`.

### The rail frame
A centered **1240px frame** with hairline vertical rails (`border-inline: 1px solid var(--rail)`)
and horizontal rules between sections. Marketing pages and public product pages (/connect) use it.

### Focus
A 2px `--focus-ring` (violet) outline with a 2px offset on everything interactive, except text
fields, which use the border and ring above. Checkboxes, radios and sliders keep the outline.

---

## 5. Marketing only

These belong on marketing pages and nowhere in a product screen:

- **Bands**: each section is a band with `--site-band-y` padding and a hairline above it, inside
  the rail frame. A section head puts the eyebrow and heading on the left and the lede on the right
  on desktop.
- **Ruled columns**: hairlines between columns on desktop.
- **Tiles**: `--site-card` fill, `--site-radius-card` (14px), an inset hairline and `--shadow-lg`.
  A linked tile glows violet on hover (`--site-violet-glow`).
- **Pills**: small rounded labels (`--radius-pill`), `--site-pill` fill, 13px, sentence case.
- **Check lists**: each line starts with a green dot in a soft green ring.
- **The plate**: one lit panel per page for the closing call to action (`--site-plate`, 20px
  radius, violet and green light behind a dot grid). Nothing in it loops.
- **The lit call to action**: the page's one primary button may carry a green glow
  (`--site-green-glow`) and a trailing arrow.
- **The eyebrow cursor**: a green block after an eyebrow that blinks three times and rests.
- **Reveals**: content may fade up once as it scrolls into view. Reduced motion turns it off.

## 6. Product only

Product screens read like Linear: dense, quiet and hairline-ruled, and unmistakably MetriFi
through the violet-black neutrals, green numbers, the square green button and the uppercase labels.

- **Density**: table rows `--ui-row` (36px), compact lists and menus `--ui-row-compact` (32px),
  controls `--ui-control` (36px) or `--ui-control-sm` (28px). Spacing on a 4px grid.
- **Panels**: `--surface` fill, `--line` border, `--ui-radius-card` (12px), `--inset-hi`. No
  shadow, no glow.
- **Tables**: column headers 12px weight 500 in `--text-tertiary`, sentence case; rows separated by
  `--line-faint`; a hovered row takes `--ui-hover`, a selected one `--ui-selected`. Numbers tabular, right-aligned when they compare.
- **Badges and chips**: 12px weight 500, sentence case, `--ui-radius-chip` (4px), a status color at
  about 12% as the fill and the full color as the text.
- **Navigation and tabs**: 13px weight 500, sentence case. The active tab is violet text on
  `--violet-12`.
- **Menus, popovers and tooltips**: `--elevated`, `--line-strong` edge, `--radius-sm`,
  `--ui-shadow-pop`. **Modals**: `--surface`, 12px radius, `--ui-shadow-modal`, `--ui-scrim`
  behind. Layers use the `--z-*` tokens.
- **Data**: the hero metric, stat numbers and every "up" change use green. Labels around them stay
  neutral. Green is never decoration and never a category color in a chart.

---

## 7. Radii, motion, elevation

- **Radii**: buttons `2px` (signature), chips `4px`, inputs and product controls `6px`, product
  cards and modals `12px`, marketing tiles `14px`, the marketing plate `20px`, pills round.
- **Motion**: `--ease-standard` for interface, `--ease-out` for entrances. Product durations 120 to
  180ms. Nothing loops. Under `prefers-reduced-motion: reduce`, turn off reveals, the eyebrow
  cursor, glows that animate and the 1px press nudge; color changes may stay.
- **Elevation**: prefer the inset highlight (`--inset-hi`) to drop shadows. Shadows are for things
  that float: menus and tooltips (`--ui-shadow-pop`), the modal (`--ui-shadow-modal`), marketing
  tiles (`--shadow-lg`).

---

## 8. Do / Don't

**Do**
- Use Inter for every heading, label and number.
- Use green for numbers, metrics and proof points on any background.
- Keep uppercase for eyebrows and field labels; sentence case everywhere else.
- Use the marketing scale on marketing pages and the product scale on product screens, through
  the recipe classes or tokens rather than sizes of your own.
- Use white-alpha hairlines for borders, and `--control-edge` for anything operable.
- Let the browser know we are dark: the system declares `color-scheme: dark`, so native controls
  render dark. Don't override it.

**Don't**
- Don't use monospace for labels, eyebrows or numbers. It is for code and copyable values.
- Don't bring back Space Grotesk, or add a second typeface.
- Don't introduce a light theme.
- Don't round the primary button past 2px.
- Don't put marketing tiles, glows, plates or reveals on a product screen, or product-dense tables
  on a marketing page.
- Don't use color for emphasis on a product screen; use weight.
- Don't reach for solid gray borders or heavy drop shadows.

---

## 9. Copy

Instrument copy is as precise as its type. Readers are bank and credit union buyers, and
inconsistent punctuation reads to them as sloppy. These rules apply to every surface, product
and marketing, and to every client site MetriFi builds.

### Punctuation
- **Headings and body copy are sentences, so they end with punctuation.** That includes section
  headings: "Get insights now with a free test drive."
- **Short copy ends with no period.** Card taglines, plan values, table cells, labels, eyebrows and
  buttons: "Sized to your institution", "Built and hosted by MetriFi".
- **Never a period in the middle of short copy.** "Stays where it is. We host your landing pages"
  is wrong. Join the clauses with a comma ("Stays where it is, with landing pages hosted by
  MetriFi") or make it a full sentence with a period at the end.
- **Siblings match.** If lines in the same set (the taglines on four plan cards, the cells in one
  column) mix trailing periods, fix all of them, not just the one you were editing.
- **No em dashes in copy.** Use a comma, a period, a colon or parentheses instead.

### Units and quantities
- **Rates take a slash with spaces:** `10,000 / month`, `12 / year`. Never "a month" or "per year"
  in a data value, and never `10,000/month` without the spaces.
- **A starting point takes a trailing `+`,** not "from" or "starts at": `$36,000+`,
  `500,000+ / month`, `24+ / year`.
- **Numbers of four digits or more take commas:** `6,000`, `$16,000`.

### Spelling
- **American English everywhere:** color, license, organization, behavior, canceled, toward.
  Dates as `Month D, YYYY` or `YYYY-MM-DD`.

---

## 10. Consuming this

See [`README.md`](README.md). Short version, by property:

| Property | Import | Context |
|----------|--------|---------|
| metrifi.com (Astro, Tailwind, shadcn) | `@metrifi/brand/app.css` + `fonts.css`; marketing classes built on `--site-*` | Marketing |
| The platform's server-rendered pages (Laravel Blade) | `@metrifi/brand` (the full bundle: fonts, tokens, base, components) | Product, plus marketing type on public pages that sell |
| A React or Vue app (shadcn or Tailwind) | `@metrifi/brand/app.css` + `fonts.css` | Product |
| devops.metrifi.com (Next.js) | `@metrifi/brand/tokens.css`, fonts self-hosted | Product |

Brand assets: `@metrifi/brand/logo.svg` and `@metrifi/brand/favicon.svg`. `specimen.html` at
the repo root shows every token and both contexts; open it in a browser.

---

## 11. What changed in v1.0.0

For a property moving up from 0.x:

1. **Type**: `--font-display` is Inter (it was Space Grotesk). Headings in `base.css` are weight
   500 (they were 700); `.title` is weight 500.
2. **Labels**: `<label>` and every eyebrow are Inter, not monospace. Rules of your own that set
   `font-family: var(--font-mono)` on a label, eyebrow, badge, tab or number should switch to
   `var(--font-body)`; keep mono on code and copyable values.
3. **Body weight**: `base.css` sets body text to 400 (it was 300).
4. **Fonts**: `fonts.css` loads Inter only, as a variable font. Space Grotesk no longer loads.
5. **Element defaults are product recipes**: body text 14px / 22px (it was 17px), `h1` 28px
   weight 500, `h2` 20px weight 600, `h3` 16px weight 500. Marketing headings use the `.site-*`
   classes.
6. **Text tokens**: `--text-primary`, `--text-secondary`, `--text-tertiary` and
   `--text-quaternary`; `--text` and `--muted` stay as aliases.
7. **Components**: checkbox and radio labels are sentence case; placeholders are
   `--text-tertiary` at full opacity; a focused input gets a violet ring; `.link` and `.foot a`
   are violet (they were green); checkboxes take `accent-color: var(--violet)`.
8. **shadcn adapter**: `--muted-foreground` reads `--text-secondary`; `--destructive-foreground`
   is dark text on the red fill (light text measured 2.74:1).
9. **New tokens and classes**: `--label-*`, `--eyebrow-*`, `--radius-pill`, `--shadow-lg`, the
   `--site-*` and `--ui-*` groups with their type recipes, `--ui-hover`, `--ui-selected`, the
   `--z-*` layers; `.eyebrow`, `.field-label`, and the recipe classes.

No palette color changed.
