# 06 — Assets

## What you need to supply

| Asset | Format | Notes |
|---|---|---|
| Logo, full colour | SVG | Royal blue on transparent, for light sections |
| Logo, reversed | SVG | White on transparent, for hull and royal sections |
| Favicon | SVG + 180px PNG | Mark only, no wordmark — it renders at 16px |
| Office photography | JPG, 2400px wide | Optional. See note below. |
| SEC registration number | text | For the credentials band |
| Registered office address | text | Footer and contact |
| Contact email and phone | text | Contact section |
| Headcount / platform figures | text | Or the scale section gets deleted |

Drop everything into `public/`. Fonts go in `public/fonts/`.

## Fonts

Download from fontshare.com (free for commercial use, no attribution required):

- **Clash Display** — weights 500, 600
- **Switzer** — weights 400, 500

Convert to woff2 and subset to Latin only. Four files total, target under 30KB each.
Self-host — do not link Fontshare's CDN, it adds a third-party request and a point
of failure on a site regulators will load.

Preload only the two weights used above the fold: Clash Display 600 and Switzer 400.

## A note on photography

This design does not need photography to work. The arc geometry carries it.

If you do have real office or team photos, the one place they earn their keep is a
full-bleed band between the standards and scale sections — a single wide image of
the actual operations floor, treated with a royal blue duotone so it sits inside the
palette rather than fighting it.

**Do not use stock photography.** Generic "diverse team pointing at a monitor" images
are the fastest way to make a licensed operating company look like it does not exist.
An arc on a blue field is more credible than a stock photo. If there are no real
photos, ship without any.

## Logo handling

The wordmark in the nav and footer should be the supplied SVG at a fixed height
(28px nav, 32px footer), never scaled by width. If the supplied logo has a lockup
with a tagline, use the mark-plus-wordmark version only and drop the tagline — it
will not be legible at 28px.

If the logo's blue does not exactly match `--ark-royal` (#1B3FB8), **change the token
to match the logo, not the other way around.** Then re-check contrast for white text
on the new value and adjust `--ark-hull` to stay two clear steps darker.
