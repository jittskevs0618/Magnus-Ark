# 05 — Deployment

## 1. GitLab

```bash
git init
git add .
git commit -m "Magnus Ark corporate site"
git remote add origin git@gitlab.com:{{your-group}}/magnusark.git
git push -u origin main
```

Add a `.gitignore` covering `node_modules/`, `dist/`, `.env`, `.astro/`, `.DS_Store`.

**Branching:** work on `develop`, merge to `main` to release. Vercel will give you a
preview URL for every branch and merge request automatically, which is how you'll
review each build stage on your phone.

## 2. Vercel

1. vercel.com → Add New → Project → **Continue with GitLab**, authorise, pick the repo
2. Vercel detects Astro. Framework preset `Astro`, build `npm run build`, output `dist`
3. Deploy. You get `magnusark.vercel.app`

Every push to `main` redeploys. Every merge request gets its own preview URL.

## 3. DNS at dot.ph

In the dot.ph control panel for `magnusark.ph`, set:

| Type | Host | Value |
|---|---|---|
| A | `@` | `76.76.21.21` |
| CNAME | `www` | `cname.vercel-dns.com` |

Then in Vercel → Project → Settings → Domains, add both `magnusark.ph` and
`www.magnusark.ph`. Set the apex as primary and let `www` redirect to it.

**Verify the A record IP against Vercel's own domain settings page at the time you
configure it.** Vercel has changed this value before and dot.ph propagation can take
several hours, so you do not want to do it twice.

TLS is automatic once DNS resolves. If Vercel is still showing "Invalid
Configuration" after 4 hours, check that dot.ph hasn't kept a parking A record
alongside yours — that is the usual cause.

## 4. Supabase — contact form (Phase 2)

Only worth doing if you want enquiries queryable rather than sitting in an inbox.
Skip it for launch if you'd rather ship.

**Table:**

```sql
create table public.enquiries (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz not null default now(),
  full_name text not null,
  email text not null,
  organisation text,
  enquiry_type text not null,
  message text not null
);

alter table public.enquiries enable row level security;
-- no policies: only the service role key can read or write
```

**Route** — `src/pages/api/contact.ts`, Astro server output with
`@astrojs/vercel/serverless`:

- Validate all fields server-side. Never trust the client.
- Insert with `SUPABASE_SERVICE_ROLE_KEY`, held in Vercel environment variables.
  **This key must never appear in client code or in the repo.** If it ends up in a
  committed `.env`, rotate it immediately.
- Rate limit by IP, 5 requests per 10 minutes.
- Add a honeypot field hidden with CSS, and reject any submission that fills it.
  This stops most bots without a CAPTCHA.
- Return `{ ok: true }` or a specific error. Match the error copy in `03-CONTENT.md`.

**Notification:** add a Supabase database webhook on insert pointing at Resend or
your SMTP provider, so a human actually sees the enquiry. A form nobody checks is
worse than a `mailto:`.

## 5. Pre-launch checklist

- [ ] Every `{{TBD}}` filled with real client-supplied values
- [ ] Compliance officer has signed off the age and exclusion statement in the footer
- [ ] Privacy notice reviewed against the Data Privacy Act of 2012 — this needs a
      lawyer, not a template
- [ ] Banned word grep on `dist/` returns nothing
- [ ] `og.jpg` renders correctly in a Facebook and LinkedIn preview debugger
- [ ] Tested on an actual mid-range Android on mobile data, not just desktop DevTools
- [ ] Analytics decision made — Vercel Analytics is cookieless and the simplest
      choice here given the privacy posture
