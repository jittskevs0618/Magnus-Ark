# Magnus Ark — Corporate Website

Single-page parallax corporate site for **Magnus Ark Inc.**, a Philippine corporation
licensed and approved by PAGCOR as a **Gaming System Administrator**.

Domain: `magnusark.ph` (registrar: dot.ph)

## How to use these docs with Claude Code

Drop this whole `docs/` folder into an empty repo, then start Claude Code in that repo and say:

> Read every file in `docs/`, in order. Then build the site described in
> `04-BUILD-SPEC.md`, following `02-DESIGN-SYSTEM.md` exactly for all visual
> decisions and `03-CONTENT.md` verbatim for all copy. Do not invent statistics,
> client names, awards, or dates. Start by giving me a build plan and a file tree
> before you write any code.

Then build in passes rather than one shot:

1. Scaffold + design tokens + hero section only. Review in browser.
2. Sections 2–4. Review.
3. Sections 5–8 + footer. Review.
4. Responsive, accessibility, performance pass.
5. Deploy (see `05-DEPLOYMENT.md`).

## The files

| File | What it covers |
|---|---|
| `01-PROJECT-BRIEF.md` | What the company is, who the site is for, tech stack, hard rules |
| `02-DESIGN-SYSTEM.md` | Palette, typography, spacing, the arc motif, parallax mechanics |
| `03-CONTENT.md` | Every word on the site — about, vision, mission, services, footer |
| `04-BUILD-SPEC.md` | Section-by-section build instructions with wireframes |
| `05-DEPLOYMENT.md` | GitLab, Vercel, Supabase, dot.ph DNS |
| `06-ASSETS.md` | What you need to supply, and where it goes |

## Two rules that override everything

1. **No gambling vocabulary anywhere.** See the banned/approved word list in `01-PROJECT-BRIEF.md`.
2. **Never mention the consumer brand.** This site is about the corporate entity only.
