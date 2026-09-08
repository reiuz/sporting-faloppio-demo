# Design

## Theme
Clay-court club. Cool white surfaces so the terracotta reads as court heat. Deep forest green as the settled shadow, honey-gold reserved for ranks, prizes and trophies (gamification only). Light base for both site and portal; portal uses a dark "court at night" header band for contrast and hype. Airy, generous spacing, Apple-style scroll reveals.

## Color (OKLCH)
- `--bg` oklch(1 0 0) pure white
- `--surface` oklch(0.985 0.004 250) cool near-white section band
- `--surface-2` oklch(0.965 0.006 250)
- `--ink` oklch(0.22 0.02 265) deep slate near-black
- `--muted` oklch(0.46 0.02 265) body-safe on white (>=4.5:1)
- `--faint` oklch(0.62 0.015 265) large text / borders labels only
- `--line` oklch(0.90 0.005 265)
- `--clay` oklch(0.56 0.17 40) primary terracotta
- `--clay-deep` oklch(0.45 0.15 38) hover/press, text-on-white safe
- `--clay-wash` oklch(0.96 0.03 45) tint band
- `--court` oklch(0.42 0.09 155) forest green accent
- `--court-deep` oklch(0.30 0.07 155)
- `--gold` oklch(0.80 0.14 88) rank/prize highlight (large/decorative only)
- `--gold-ink` oklch(0.52 0.11 80) gold as readable text on white
- `--night` oklch(0.20 0.03 260) dark portal header ground
- Semantic: win `--court`, loss `--clay-deep`.

## Typography
- Display: "Bricolage Grotesque", 700/800 — headlines, hero, section titles.
- Body/UI: "Hanken Grotesk", 400/500/600/700.
- Mono/numeric: "JetBrains Mono", 500/700 — rankings, scores, stats, ELO, court times (tabular-nums).
- Scale: fluid clamp() on headings, >=1.25 ratio. Hero max <=6rem. Display letter-spacing >= -0.03em. `text-wrap: balance` on h1-h3.

## Motion
- IntersectionObserver reveal: opacity 0->1 + translateY(16px)->0, ease-out-expo, staggered per group. Content visible by default; reveal only enhances.
- Hero parallax (transform on scroll), count-up numbers for stats, animated ladder bars, tab crossfades, booking-slot hover.
- Curves: cubic-bezier(.16,1,.3,1). No bounce/elastic.
- `@media (prefers-reduced-motion: reduce)`: reveals become instant, parallax off, count-ups show final value.

## Layout
- Max width ~1180px, fluid gutters via clamp.
- Grids: repeat(auto-fit, minmax(...)) where cards are the right affordance; avoid uniform card walls.
- Portal: app shell, sidebar (desktop) / bottom tab bar (mobile), dark header band, light content.
- z-scale: base < sticky-header < bottom-nav < dropdown < modal-backdrop < modal < toast.

## Imagery
Unsplash clay-court / tennis photography, verified IDs, `?auto=format&fit=crop&w=...&q=80`. Hero full-bleed court. Alt text specific ("terra rossa indoor sotto i fari").

## i18n
JS dictionary keyed by `data-i18n`; IT default, EN toggle; choice persisted in localStorage; `<html lang>` updated.
