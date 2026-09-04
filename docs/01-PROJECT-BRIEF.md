# 01 — Project Brief

## The company

Magnus Ark Inc. is a Philippine corporation licensed and approved by the Philippine
Amusement and Gaming Corporation (PAGCOR) as a **Gaming System Administrator (GSA)**.

Behind the license sits an operating company that provides three service lines to
licensed digital entertainment platforms:

- **Technology and product development** — platform engineering, integrations, systems administration
- **Business process outsourcing** — customer experience, account verification, payments operations, risk and integrity monitoring
- **Marketing services** — brand, performance marketing, CRM and lifecycle, creative production

Magnus Ark is the corporate entity. It does not sell to consumers. It is the company
behind licensed platforms, not a platform itself.

## Who the site is for

In priority order:

1. **Regulators and auditors** — PAGCOR, banking partners, KYC/AML reviewers doing due diligence
2. **Commercial partners** — content providers, payment processors, technology vendors evaluating credibility
3. **Talent** — senior hires in engineering, compliance, and operations
4. **Investors and corporate counterparties**

None of these people are players. The site should read like a licensed operating
company's corporate site, not like a product marketing page.

## What the site must accomplish

- Establish that Magnus Ark is real, licensed, and institutionally serious
- Explain what the company actually does, in plain terms
- State vision and mission
- Give a single, credible way to make contact
- Load fast on Philippine mobile connections

## Scope

Single page, ~8 scroll sections, parallax-driven. No CMS. No blog. No login.
One contact form.

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Framework | **Astro 5** | Ships zero JS by default. Components are basically HTML files — familiar if you last wrote HTML and jQuery. |
| Styling | **Tailwind CSS 4** via `@tailwindcss/vite` | Design tokens live in one CSS file |
| Motion | Native CSS scroll-driven animations + a ~40-line vanilla JS fallback | No GSAP, no ScrollMagic, no jQuery |
| Repo | **GitLab** | As requested |
| Hosting | **Vercel** | Connects to GitLab natively, free tier is fine, good PH edge latency |
| Forms | **Supabase** (Postgres + RLS) — Phase 2 | Overkill for one form, but you asked, and it gives a queryable inbox |
| Fonts | Self-hosted woff2 | No Google Fonts request, faster and privacy-clean |

Do not add: React, a component library, an animation library, a carousel library,
or an icon package. Icons are inline SVG, hand-written, four of them at most.

## Hard content rules

### Never use these words

`casino`, `gambling`, `gamble`, `bet`, `betting`, `bettor`, `wager`, `wagering`,
`odds`, `jackpot`, `slots`, `punter`, `stake`, `payout`, `win big`, `lucky`,
`house edge`, `deposit bonus`

### Use these instead

| Instead of | Write |
|---|---|
| online casino | regulated digital entertainment platform |
| gambling industry | regulated gaming and entertainment sector |
| players/bettors | players, users, or customers |
| betting platform | entertainment platform, gaming system |
| casino operations | platform operations |
| gaming (as an activity) | digital entertainment |

`gaming` is acceptable **only** in the formal regulatory sense — "Gaming System
Administrator", "the regulated gaming sector", "gaming systems". Never as a verb,
never as a description of what players do.

### Never mention

- The consumer brand name or any consumer brand name
- Specific revenue, headcount, player counts, or GGR figures
- Award names, client logos, or partner logos that have not been supplied
- Any invented date, license number, or executive name

If a number or name is needed and has not been provided, use a clearly marked
placeholder like `{{TBD: headcount}}` rather than inventing one. Do not ship
invented facts into a site that regulators will read.

## Tone

Institutional, plain, unhurried. Short declarative sentences. First person plural.
No exclamation marks. No "revolutionizing", "cutting-edge", "world-class",
"seamless", "empowering", "unlock", "leverage", "solutions provider". No em-dash
asides. Write the way a licensed operating company writes to its regulator.
