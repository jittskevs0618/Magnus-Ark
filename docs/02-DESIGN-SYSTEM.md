# 02 — Design System

## The idea

The name is the design. An **ark** is a structure built to carry something valuable
through conditions it has to survive. It is also an **arc** — a load-bearing curve.

So the site is built from one geometric primitive: **the arc**. A vault, a hull
section, a keel line. Arcs frame sections, cut section boundaries, and form the
parallax depth planes. Nothing else decorates the page.

This gives Magnus Ark a look that a generic corporate template cannot accidentally
arrive at: heavy structural curves against flat royal blue, no cards, no shadows,
no rounded rectangles.

**What this is not:** not a card grid, not a gradient-mesh hero, not glassmorphism,
not a dark page with a neon accent, not a "trusted by" logo carousel.

## Palette

Six values. Do not add a seventh.

```css
--ark-hull:   #071026;  /* deepest ground — hero and footer */
--ark-royal:  #1B3FB8;  /* the brand royal blue — primary surface and fills */
--ark-lift:   #3C63E8;  /* interaction, focus rings, live states only */
--ark-paper:  #F2F4F9;  /* light sections */
--ark-white:  #FFFFFF;  /* type on blue, and pure surfaces */
--ark-brass:  #C9A24A;  /* credential marks ONLY — see rule below */
```

**Brass rule.** Brass appears exactly three times on the page: the license badge
rule in the hero, the arc keyline in the credentials band, and the top border of
the footer. Nowhere else. Not on buttons, not on headings, not on hover. Its scarcity
is what makes the license read as authoritative rather than decorative.

**Section rhythm.** Alternating value, not alternating hue:

```
hero        hull (darkest)
about       paper
credentials royal
vision      hull
services    paper
standards   royal
scale       paper
contact     hull
```

Never place two `paper` sections adjacent. The parallax arcs read against the value
change at each boundary.

## Typography

Two families from Fontshare, self-hosted as woff2 (see `06-ASSETS.md`).

**Display — Clash Display.** Geometric grotesque with flat terminals and a slightly
compressed frame. Used at 600 weight for section headings, 500 for the wordmark line.
Set headline tracking to `-0.03em`. It is architectural without being cold, which is
exactly the register.

**Body — Switzer.** Neutral, high x-height, excellent at 16–19px on Philippine mobile
screens. 400 for body, 500 for labels.

Fallbacks if Fontshare is unavailable: `Archivo` (display) and `Inter` (body). Do not
default to Inter for both.

### Type scale

Major third (1.25), based at 18px. Fluid via `clamp()`.

| Role | Size | Weight | Tracking | Leading |
|---|---|---|---|---|
| Hero line | `clamp(2.75rem, 7vw, 6rem)` | 600 | -0.035em | 0.95 |
| Section heading | `clamp(2rem, 4vw, 3.25rem)` | 600 | -0.03em | 1.05 |
| Lead paragraph | `clamp(1.25rem, 2vw, 1.5rem)` | 400 | -0.01em | 1.45 |
| Body | `1.125rem` | 400 | 0 | 1.6 |
| Service title | `1.5rem` | 500 | -0.02em | 1.2 |
| Meta / caption | `0.9375rem` | 500 | 0.01em | 1.4 |

Body measure: **62–68 characters.** Never wider.

### Typographic prohibitions

- No ALL-CAPS eyebrow labels above headings. Section identity comes from the arc
  geometry and the value change, not from a tracked-out kicker.
- No accenting a single word in a headline in a different colour or weight.
- No `→` appended to link text.
- No monospace anywhere.
- Sentence case for every heading and button. Not Title Case.

## The arc motif

One SVG primitive, reused at four scales.

**The keel arc.** A single arc segment, 2400 units wide, rising 340 units, drawn as
either a 1.5px keyline or a solid fill.

```
        ╭──────────────────────────╮
   ╭────╯                          ╰────╮
 ──╯                                    ╰──
```

Four applications:

1. **Hull arc** — full-bleed solid fill in `--ark-royal`, sits behind the hero
   content, moves slowest in parallax. This is the signature element of the site.
2. **Section cut** — the boundary between two sections is an arc, not a straight
   line. The lower section's background rises into the upper one.
3. **Keyline arc** — 1.5px stroke at 25% opacity, tiled twice at different scales
   as the far parallax plane.
4. **Numeral arc** — in the scale section only, an arc underlines each figure.

Generate these as inline SVG with `preserveAspectRatio="none"` so they stretch to
viewport width without the stroke distorting badly. Keep the path in a single shared
Astro component and pass scale/opacity as props.

## Parallax mechanics

Three depth planes. Ratios are relative to scroll distance:

| Plane | Content | Speed |
|---|---|---|
| Far | keyline arcs, section cut arcs | 0.25 |
| Mid | hull arc fill, section background | 0.6 |
| Near | text, buttons, form | 1.0 (normal flow) |

### How to implement

**Primary path — CSS scroll-driven animations.** No JS.

```css
@supports (animation-timeline: view()) {
  .plane-far {
    animation: drift linear both;
    animation-timeline: view();
    animation-range: cover 0% cover 100%;
  }
  @keyframes drift { to { transform: translate3d(0, -18vh, 0); } }
}
```

**Fallback — one small script.** For Safari versions without `animation-timeline`,
use a single `requestAnimationFrame` loop reading `scrollY` once per frame and writing
`transform: translate3d()` to at most 8 elements. Never read layout inside the loop.
Never attach a raw `scroll` listener that writes styles directly.

### Motion rules

- **Only the arcs move on scroll.** Text never fades or slides up on entry. Every
  section fading in on scroll is the single clearest tell of a generated page, and
  it makes the site feel slow on mobile.
- One orchestrated moment only: on first load, the hull arc rises into position over
  700ms with `cubic-bezier(0.22, 1, 0.36, 1)`, and the hero line is already visible
  beneath it. That is the whole entrance.
- Hover: colour change only, 120ms. No lift, no scale, no shadow.
- `@media (prefers-reduced-motion: reduce)` — disable all parallax, render every
  plane at its final position, keep the layout identical.
- **Disable parallax entirely below 768px.** Arcs render static. Mobile scroll
  performance matters more than the effect.

## Layout

- Grid: 12 columns, 1240px max, 24px gutters. 20px page padding on mobile.
- **Alignment: left, consistently.** Headings, body, and section content all align
  to the same left rail. Centred text appears exactly once, in the contact section.
  Left alignment reads as institutional; centred everything reads as a landing page.
- Vertical rhythm: sections are `clamp(96px, 12vh, 180px)` of padding, top and bottom.
  Arc cuts overlap this padding, they do not add to it.
- **Zero border-radius on all containers.** The curves in this design are structural
  arcs, not rounded corners. The only radius on the page is 2px on form inputs.
- No box-shadows. Depth is created by value and by the parallax planes.

## Quality floor

- Responsive to 320px
- Visible focus ring: 2px `--ark-lift`, 2px offset, on every interactive element
- All text meets WCAG AA against its background. Check `--ark-white` on
  `--ark-royal` and `--ark-brass` on `--ark-hull` specifically.
- Images: `loading="lazy"`, explicit width/height, AVIF with WebP fallback
- Lighthouse target: 95+ performance on mobile
- No layout shift from font loading — use `font-display: swap` with a metric-matched
  fallback stack
