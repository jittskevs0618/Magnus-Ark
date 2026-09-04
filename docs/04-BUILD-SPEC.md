# 04 — Build Spec

## Scaffold

```bash
npm create astro@latest magnusark -- --template minimal --typescript strict
cd magnusark
npx astro add tailwind
npm i -D sharp
```

## File tree

```
magnusark/
├── docs/                        ← these files
├── public/
│   ├── fonts/                   ← ClashDisplay-{500,600}.woff2, Switzer-{400,500}.woff2
│   ├── logo.svg
│   ├── logo-white.svg
│   ├── og.jpg
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── Nav.astro
│   │   ├── ArcCut.astro         ← the shared arc primitive
│   │   ├── Plane.astro          ← parallax depth wrapper
│   │   └── sections/
│   │       ├── Hero.astro
│   │       ├── About.astro
│   │       ├── Credentials.astro
│   │       ├── VisionMission.astro
│   │       ├── Services.astro
│   │       ├── Standards.astro
│   │       ├── Scale.astro
│   │       └── Contact.astro
│   ├── layouts/
│   │   └── Base.astro
│   ├── pages/
│   │   ├── index.astro
│   │   ├── privacy.astro
│   │   ├── terms.astro
│   │   └── responsible-play.astro
│   ├── scripts/
│   │   └── parallax.ts          ← fallback only, ~40 lines
│   └── styles/
│       └── global.css           ← @theme tokens from 02-DESIGN-SYSTEM.md
├── astro.config.mjs
└── package.json
```

Build `ArcCut.astro` and `Plane.astro` **first**. Every section depends on them.

### ArcCut.astro

Props: `variant` (`fill` | `keyline`), `color`, `flip` (boolean), `height` (px),
`opacity`. Renders one inline SVG, absolutely positioned, `preserveAspectRatio="none"`,
`aria-hidden="true"`, `pointer-events: none`. One path, no gradients, no filters.

### Plane.astro

Props: `depth` (`far` | `mid` | `near`). Wraps a slot in a div with the right
`animation-timeline` CSS and a `data-plane` attribute the fallback script reads.
`near` renders a plain div with no transform — normal document flow.

---

## Nav

Fixed, 68px tall, transparent over the hero, gains a `--ark-hull` background at 92%
opacity with backdrop blur once scrolled past 80vh. Logo left. Four anchor links
right: `About`, `Services`, `Standards`, `Contact`. Mobile: logo plus a single
`Contact` link, no hamburger — four anchors do not justify a drawer.

---

## Sections

### 1. Hero — `100vh`, min 640px, `--ark-hull`

```
┌──────────────────────────────────────────────┐
│ [logo]                    about services ... │
│                                              │
│                                              │
│  Built to carry regulated                    │
│  digital entertainment                       │
│  in the Philippines.                         │
│                                              │
│  ────────────────────────  ← brass rule, 64px│
│  Magnus Ark is a Philippine corporation      │
│  licensed and approved by PAGCOR as a        │
│  Gaming System Administrator.                │
│                                              │
│      ╭────────────────────────────╮          │
│   ╭──╯   HULL ARC — royal fill    ╰──╮       │  ← mid plane
│ ──╯                                  ╰──     │
└──────────────────────────────────────────────┘
```

Hero line sits at 12 columns wide max, left rail, vertically centred in the upper
60%. Hull arc occupies the bottom ~38%, rises 60px on load. Keyline arcs sit behind
it on the far plane at 25% opacity. Scroll cue bottom-left, 15px, fades out after
first scroll input.

The hero line breaks over three lines on desktop. Force it with `<br>` — do not
leave it to the browser, the break points carry the rhythm.

### 2. About — `--ark-paper`, arc cut rising from below

```
┌──────────────────────────────────────────────┐
│ ╰──────────────╮        ╭─────────────╯      │ ← arc cut from hull
│                                              │
│ Who we are                                   │
│                                              │
│ [lead paragraph, 8 cols]     │  [empty 4]    │
│                                              │
│ [body paragraph 1, 7 cols]                   │
│ [body paragraph 2, 7 cols]                   │
└──────────────────────────────────────────────┘
```

The empty right column is deliberate. Do not fill it with an image or a stat block.

### 3. Credentials — `--ark-royal`, white type

```
┌──────────────────────────────────────────────┐
│ Licensed and accountable                     │
│ [body, 6 cols]                               │
│                                              │
│  ──────────────  brass keyline arc, 1.5px    │
│                                              │
│  Regulator        Authority       Registered │
│  PAGCOR           Gaming System   {{TBD}}    │
│                   Administrator              │
│                                              │
│  Accreditation details available on request. │
└──────────────────────────────────────────────┘
```

Three items on a baseline row, divided by 1px white rules at 20% opacity. Not cards.
Label above value, label at meta size, value at service-title size.

### 4. Vision and mission — `--ark-hull`

Two blocks stacked, not side by side. Vision first at ~7 columns, then 96px gap,
then mission at ~8 columns, indented one column to the right of vision. The stagger
does the structural work; no dividers, no icons, no boxes.

Heading `Where we are going` sits above both. Label each block with `Vision` and
`Mission` in body weight at body size — these are content, not eyebrow labels.

### 5. Services — `--ark-paper`

Four items, full-width rows stacked vertically, each separated by a 1px rule in
`--ark-royal` at 15%. Not a 2×2 card grid.

```
┌──────────────────────────────────────────────┐
│ What we do                                   │
│ [lead, 6 cols]                               │
│──────────────────────────────────────────────│
│ Platform technology  │ [description, 6 cols] │
│──────────────────────────────────────────────│
│ Operations           │ [description, 6 cols] │
│──────────────────────────────────────────────│
│ Marketing            │ [description, 6 cols] │
│──────────────────────────────────────────────│
│ Compliance and       │ [description, 6 cols] │
│ player protection    │                       │
│──────────────────────────────────────────────│
└──────────────────────────────────────────────┘
```

Title left at 4 columns, description right at 6, one column gutter between.
Stack to single column below 900px. No numbering — these are four parallel
capabilities, not a sequence.

### 6. Standards — `--ark-royal`, white type

Four commitments in a 2×2 arrangement. Lead sentence in 500 weight, remainder in 400,
same size, same colour — the weight change carries the hierarchy without a separate
heading level. 1px white rules at 20% between rows and columns.

### 7. Scale — `--ark-paper`

Four figures on one row. Numeral in display face at `clamp(3rem, 6vw, 5rem)`, small
brass arc underneath each, label below in meta size. **Delete this section if real
figures are not supplied.**

### 8. Contact — `--ark-hull`

The one centred block on the page. Form at 6 columns centred, direct contact details
below in a single centred line. Inputs: transparent background, 1px bottom border in
white at 30%, border goes `--ark-lift` on focus. 2px radius, nothing more. Labels
above inputs, always visible — no placeholder-as-label.

Submit posts to `/api/contact` (see `05-DEPLOYMENT.md`). Until Supabase is wired,
render the form disabled with a `mailto:` link beneath it rather than a form that
silently fails.

### Footer — `--ark-hull`, 1px brass top border

Wordmark, regulatory line, three links, age statement, copyright. Left aligned.

---

## Build order

Do not attempt this in one pass. Stop after each stage and show a screenshot.

1. Scaffold, `global.css` tokens, fonts loading, `ArcCut` + `Plane` components
2. Nav + Hero, desktop only, with the load animation working
3. Sections 2–4
4. Sections 5–8 + footer
5. Responsive: 1440 / 1024 / 768 / 390 / 320
6. Accessibility: keyboard path, focus rings, reduced motion, contrast audit
7. Performance: font subsetting, image formats, Lighthouse mobile
8. Legal pages (privacy, terms, responsible play) as plain typographic pages

## Definition of done

- No horizontal scroll at any width from 320px to 2560px
- Full keyboard path with visible focus on every interactive element
- `prefers-reduced-motion` renders a static, correct page
- Zero console errors, zero 404s
- Lighthouse mobile ≥95 performance, 100 accessibility
- Every `{{TBD}}` either filled or visibly flagged
- Grep the built HTML for the banned word list in `01-PROJECT-BRIEF.md` — zero hits
