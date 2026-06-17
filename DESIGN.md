# MetriFi "Instrument" Design System

The visual language for MetriFi 2.0 — product-only, dark-only. Named **Instrument**:
precise, technical, and quietly confident, like a well-made measuring tool. This document
is the written spec; [`css/tokens.css`](css/tokens.css) is the machine-readable one. When the
two disagree, the tokens win and this doc is wrong — fix it.

> **Scope.** Dark mode is the *only* mode. There is no light theme. The system is for MetriFi
> **product** surfaces (apps, connectors, the gateway, the IdP). Marketing sites may diverge.

---

## 1. Voice

Instrument reads like an instrument panel, not a brochure:

- **Monospace labels, uppercase, letter-spaced** — eyebrows, field labels, metadata, status. This
  is the system's signature. `// set it up`, `MCP CONNECTOR`, `NAME`, `STREAMABLE HTTP`.
- **Space Grotesk for display**, tight tracking, set big and bold.
- **Inter at weight 300 for body** — light, roomy line-height, never heavy.
- **Restraint with color.** The canvas is near-black violet; accents are used like punctuation,
  not paint. One accent per context (see §2).

---

## 2. Color

### Surfaces (dark-only)
| Token | Hex | Use |
|-------|-----|-----|
| `--canvas` | `#0F0A1A` | Page background |
| `--surface` | `#1A1230` | Cards, panels |
| `--elevated` | `#241845` | Raised surfaces, hovers |

### Accents — the one rule that matters
**Violet is a canvas accent. Green is a surface accent.** They swap based on what's behind them.

- On the **canvas** (page background), emphasis and links are **violet** (`--violet #A074FF`).
- On a **surface** (card/panel), emphasis and accents are **green** (`--green #7DD4AA`).

This is why `h1 em` is violet but `.mf-surface h2 em` is green. When you put a card on the page,
add `.mf-surface` so the accent flips correctly. Getting this backwards is the most common way to
break the brand.

| Token | Hex | |
|-------|-----|-|
| `--violet` / `--violet-hover` | `#A074FF` / `#B088FF` | Canvas accent, focus ring |
| `--green` / `--green-hover` | `#7DD4AA` / `#93E0BB` | Surface accent, **primary CTA fill** |

### Status
| Token | Hex | Use |
|-------|-----|-----|
| `--gold` | `#D4B84A` | Warnings, pending |
| `--red` | `#E85D56` | Errors, destructive |

### Text
| Token | Hex | Use |
|-------|-----|-----|
| `--text` | `#E8E4F0` | Primary |
| `--muted` | `#A89FC0` | Secondary, labels, captions |
| `--dark-text` | `#0F0A1A` | Text on green/violet fills |

### Hairlines & washes
Borders are **white at low alpha**, never solid grays: `--line-faint` (.06) → `--line` (.08) →
`--line-soft` (.10) → `--line-strong` (.14). Tinted backgrounds use the wash tokens
(`--violet-08/12/20`, `--green-08/12/20`) — e.g. selection is `--violet-20`.

---

## 3. Typography

| Role | Family | Weight | Notes |
|------|--------|--------|-------|
| Display (`h1`–`h3`) | `--font-display` Space Grotesk | 700 | `letter-spacing: -0.02em`, `line-height: 1.12` |
| Body | `--font-body` Inter | **300** | `1.0625rem` / `1.6`. Light is the default — resist bumping it. |
| Labels / eyebrows / metadata | `--font-mono` | — | `11–13px`, `UPPERCASE`, `letter-spacing: 0.08–0.16em` |

**Headline scale** (fluid): `h1` ≈ `clamp(2.3rem, 4.4vw, 3.5rem)` at `line-height: 1.05`;
`h2` ≈ `clamp(1.7rem, 3vw, 2.3rem)`. Use `text-wrap: pretty` / `balance` on long lines.

**Eyebrows** are mono, uppercase, and colored by context — violet on canvas (`klabel`), muted
when secondary, green when calling out something live. The `// ` prefix marks a section label.

---

## 4. Components

### Buttons
- **Near-square corners** — `--radius-button: 2px`. This squared-off button is a brand signature;
  do not round it.
- **Primary CTA**: green fill, canvas-colored text, **hover flips to violet**. (`--cta` → `--cta-hover`.)
- **Ghost / secondary**: transparent, violet text + 1.5px violet border, brightens on hover.
- `:active` nudges down 1px. Transitions use `--ease-standard` at ~140ms.

### Cards / panels
`--surface` background, `--line` border, `--radius-md` (12px), `--inset-hi` top highlight. Mark
them `.mf-surface` so the accent flips to green inside.

### Inputs
`--canvas` background, `--line-strong` border, `--radius-sm` (6px). Focus = violet border, no
default outline. Labels above inputs are **mono uppercase** (see Voice).

### The rail frame (marketing/hub layouts)
A centered **1240px frame** with hairline vertical rails (`border-inline: 1px solid var(--rail)`)
and horizontal `<hr>` rules between sections. Section padding is fluid
(`clamp(48px, 7vw, 88px)`). This gives the "spec sheet" feel.

---

## 5. Radii, motion, elevation

- **Radii** — buttons `2px` (signature), inputs/small `6px`, cards `12px`, large `16px`.
- **Motion** — `--ease-standard` for UI, `--ease-out` for entrances. Keep durations short (120–180ms).
- **Elevation** — prefer the inset highlight (`--inset-hi`) over drop shadows. `--shadow-md` exists
  for genuinely floating elements (menus, modals) only.

---

## 6. Do / Don't

**Do**
- Flip the accent with `.mf-surface` whenever content sits on a card.
- Keep body text at weight 300.
- Use mono uppercase for every label, eyebrow, and piece of metadata.
- Use white-alpha hairlines for borders.

**Don't**
- Don't introduce a light theme.
- Don't round buttons past 2px.
- Don't use violet on a surface or green on the bare canvas as the *primary* accent — that's the
  one rule (§2).
- Don't reach for solid gray borders or heavy drop shadows.
- Don't add a second accent color to a single context.

---

## 7. Consuming this

See [`README.md`](README.md). Short version: import `@metrifi/brand/tokens.css` for just the
variables, or `@metrifi/brand` (the full bundle) for tokens + base + components. Brand assets are
`@metrifi/brand/logo.svg` and `@metrifi/brand/favicon.svg`.
