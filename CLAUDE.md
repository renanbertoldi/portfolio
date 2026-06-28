# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file HTML portfolio for Renan Granemann Bertoldi, a Brazilian finance executive. All HTML, CSS, and JS live in `index.html`. No build step, no dependencies beyond Google Fonts (loaded via CDN).

- **Live URL:** https://renanbertoldi.github.io/portfolio/
- **Git remote:** https://github.com/renanbertoldi/portfolio.git (branch: `main`)
- **Deployment:** GitHub Pages from `main` branch root. Every push to `main` deploys automatically.

## Running locally

Open `index.html` directly in a browser, or serve with any static server:

```bash
npx serve .
# or
python -m http.server 8080
```

No build, no transpilation, no package manager.

## Design system (do not deviate)

**Colors (CSS variables in `:root`):**
- `--bg: #0D1520` — page background (navy)
- `--surface: #141E2D` — elevated cards, hero background
- `--surface-hi: #1B2740` — hover states
- `--gold: #C4973B` — accent, headings, numbers
- `--text: #F0ECE4` — primary text
- `--text-60` / `--text-35` — muted text at 60% / 35% opacity
- `--border: rgba(240,236,228,.07)` — hairline borders, grid gaps

**Fonts:**
- `Outfit` (300/400/500/600/700) — body, headings, labels
- `JetBrains Mono` (400/500) — eyebrows, KPI numbers, period labels, monospace tags

**Grid pattern:** 1px gap + `background: var(--border)` on the grid container + `outline: 1px solid var(--border)` gives the hairline-border grid effect used throughout (KPI, bank logos, events, DISC forces).

## Logo filter system

Three treatments for `<img>` tags inside `.bank-logo-wrap`:

| Class | When to use | CSS |
|---|---|---|
| *(none — default)* | Logos with any solid-color background (Itaú orange, XP yellow, C6 yellow) | `filter: brightness(0) invert(1); opacity: .55` |
| `logo-screen` | Logos on dark/transparent background | `filter: none; mix-blend-mode: screen` |
| `logo-screen-gray` | Logos on colored background that need grayscale first (Caixa) | `filter: grayscale(1) contrast(10); mix-blend-mode: screen` |

## Mobile-only visibility

`.bank-desktop-only` hides an element on screens ≤768px (`display: none`). Used for Regions Bank in the international grid — RAK Bank is kept visible on mobile in its place.

## Scroll animations (JS, bottom of `<body>`)

Three `IntersectionObserver` instances:

1. **`io`** — adds `.vis` to `.fade-up` elements on scroll. Opacity 0→1 + translateY 28px→0. Staggered with `.d1`–`.d4` delay classes.
2. **`kpiIO`** — adds `.kpi-lit` to `.kpi-cell` on scroll, triggering the gold underline animation (`::after` width 0→100%).
3. **`discIO`** — reads `data-pct` attribute on `.disc-bar-row`, sets `--pct` CSS variable, adds `.vis` to animate bar fill width.

Hero `.fade-up` elements trigger immediately via `requestAnimationFrame` (not on scroll).

## Section structure (nav anchors)

`#impacto` → `#internacional` → `#bancos` → `#eventos` → `#dados` → `#perfil` → `#carreira` → `#formacao`

Active nav link is set by a fourth `IntersectionObserver` (`navIO`) watching `section[id]` elements.

## Assets

All assets are in `assets/`. Naming conventions:
- `bank-*.png` — bank logos
- `event-*.png` — event logos
- `tool-*.png` — software tool logos
- `dash-*.png` / `dash-*.jpeg` — dashboard screenshots
- `flag-*.png` — country flags
- `hero-photo.png` — hero portrait

**Important:** GitHub Pages on Linux is case-sensitive. File names in HTML must exactly match filenames on disk (lowercase).

## Committing

```bash
git add index.html assets/<file>   # stage specific files
git commit -m "message"
git push
```

Always stage by filename, not `git add .`, to avoid accidentally committing unrelated files.
