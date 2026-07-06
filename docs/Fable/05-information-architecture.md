# 05 — Information Architecture

> **Purpose:** sitemap, navigation, URL/i18n structure, and the RBAC access map for all three surfaces. Inherits `00-foundation.md` (locales, roles) and `04-prd.md` (requirement IDs). `07-user-flows.md` cites these routes verbatim.

---

## 1. Sitemap

One Next.js app, three route groups (00 §4.1). Romanian at root, English under `/en` (00 §4.4).

```
PUBLIC (visitor)
├── /                          Home
├── /mission                   Mission & who we are
├── /membership                Tiers & benefits comparison
├── /sponsors                  Sponsors showcase
├── /fleet                     Fleet showcase
├── /contact                   Contact & form
├── /join                      Join entry point (tier preselect)
├── /legal/privacy             Privacy policy
├── /legal/terms               Terms of membership
├── /legal/cookies             Cookie notice
├── /legal/accessibility       Accessibility statement (WCAG 2.2 AA target)
└── /verify/{token}            Member-card verification (partner-facing)

AUTH (visitor)
├── /login
├── /register
└── /reset-password            (+ confirmation step)

MEMBER PORTAL (role: member)
├── /portal                    Dashboard
├── /portal/apply              Membership application (pre-member state)
├── /portal/membership         Current membership + history
│   ├── /portal/membership/pay       Payment method & execution
│   ├── /portal/membership/renew     Renewal
│   └── /portal/membership/upgrade   Tier upgrade
├── /portal/payments           Payment history & confirmations
├── /portal/card               Digital member card
├── /portal/benefits           Benefits catalog
├── /portal/announcements      Announcements feed
├── /portal/profile            Profile view/edit
└── /portal/settings           Communication preferences + GDPR self-service

ADMIN CRM (roles: staff, admin — Romanian-only)
├── /admin                     Dashboard & action queues
├── /admin/members             Member list  → /admin/members/{id}
├── /admin/flight-schools      → /admin/flight-schools/{id}
├── /admin/associations        → /admin/associations/{id}
├── /admin/aerodromes          → /admin/aerodromes/{id}
├── /admin/sponsors            → /admin/sponsors/{id}
├── /admin/contracts           → /admin/contracts/{id}
├── /admin/benefits            → /admin/benefits/{id}
├── /admin/campaigns           → /admin/campaigns/{id}
├── /admin/templates           Email templates
├── /admin/send-log            Automated + campaign email log
├── /admin/fleet               → /admin/fleet/{id}
├── /admin/users               Staff & roles            [admin]
├── /admin/settings            Club settings            [admin]
└── /admin/audit               Audit log                [admin]

API (machine)
├── /api/webhooks/stripe       Stripe webhook (PLT-009)
└── /api/cron/daily            Scheduled job endpoint (PLT-005)
```

## 2. Route table

Access levels: **V** visitor (public) · **M** member · **S** staff · **A** admin. Detail-route `{id}` = UUID; `{token}` = 22-char card token (00 §6).

| Route | Page | Access | Primary PRD IDs |
|-------|------|--------|-----------------|
| `/` | Home | V | PUB-001 |
| `/mission` | Mission | V | PUB-002 |
| `/membership` | Tier comparison | V | PUB-003, PUB-004, PUB-005 |
| `/sponsors` | Sponsors | V | PUB-006 |
| `/fleet` | Fleet showcase | V | PUB-007 |
| `/contact` | Contact | V | PUB-008 |
| `/join` | Join entry | V | PUB-009 |
| `/legal/privacy` · `/legal/terms` · `/legal/cookies` | Legal | V | PUB-010 |
| `/legal/accessibility` | Accessibility statement | V | PUB-015 |
| `/verify/{token}` | Card verification | V | PUB-013 |
| `/login` | Login | V | MEM-003 |
| `/register` | Registration | V | MEM-001 |
| `/reset-password` | Password reset | V | MEM-004 |
| `/portal` | Member dashboard | M | MEM-008 |
| `/portal/apply` | Application | M (pre-member) | MEM-002 |
| `/portal/membership` | Membership view | M | MEM-011 |
| `/portal/membership/pay` | Payment | M | MEM-005, MEM-006 |
| `/portal/membership/renew` | Renewal | M | MEM-012 |
| `/portal/membership/upgrade` | Upgrade | M | MEM-013 |
| `/portal/payments` | Payment history | M | MEM-014 |
| `/portal/card` | Member card | M | MEM-015, MEM-016 |
| `/portal/benefits` | Benefits catalog | M | MEM-017, MEM-018 |
| `/portal/announcements` | Announcements | M | MEM-020 |
| `/portal/profile` | Profile | M | MEM-009, MEM-010 |
| `/portal/settings` | Preferences & privacy | M | MEM-019, MEM-021, MEM-022 |
| `/admin` | CRM dashboard | S | ADM-001, ADM-002 |
| `/admin/members` (+`/{id}`) | Members | S | ADM-003 – ADM-011, ADM-035 |
| `/admin/flight-schools` (+`/{id}`) | Flight schools | S | ADM-012, ADM-016 |
| `/admin/associations` (+`/{id}`) | Associations | S | ADM-013, ADM-016 |
| `/admin/aerodromes` (+`/{id}`) | Aerodromes | S | ADM-014, ADM-016 |
| `/admin/sponsors` (+`/{id}`) | Sponsors | S | ADM-015, ADM-016 |
| `/admin/contracts` (+`/{id}`) | Contracts | S | ADM-017 – ADM-020 |
| `/admin/benefits` (+`/{id}`) | Benefits | S | ADM-021, ADM-022 |
| `/admin/campaigns` (+`/{id}`) | Campaigns | S | ADM-024 – ADM-027 |
| `/admin/templates` | Email templates | S | ADM-023 |
| `/admin/send-log` | Send log | S | ADM-026, ADM-028 |
| `/admin/fleet` (+`/{id}`) | Fleet | S | ADM-029 – ADM-031 |
| `/admin/users` | Staff management | **A** | ADM-032 |
| `/admin/settings` | Club settings | **A** | ADM-033 |
| `/admin/audit` | Audit log | **A** | ADM-034 |
| `/api/webhooks/stripe` | Webhook | machine (signature) | PLT-009 |
| `/api/cron/daily` | Cron | machine (secret header) | PLT-005, PLT-006 |

### URL & i18n rules

- Locale prefix applies to **public and portal** routes: `/membership` (ro) ↔ `/en/membership`. Path segments are English and unlocalized in both locales (00 §4.4).
- `/admin/*` renders `ro` only and takes no `/en` prefix (PLT-003 AC2).
- `/verify/{token}` is locale-less; it renders bilingually on one screen (08 §6.5).
- No public entity slugs in v1 — partners/aircraft have no public detail pages; admin uses UUIDs.

## 3. Navigation model

**Public header** (08 §5): logo → `/` · Mission · Membership · Sponsors · Fleet · Contact · locale switcher · Login · **Join** (accent button). Footer: nav repeat, legal links, contact details, sponsor strip (Gold), Plausible-friendly no-tracker note.

**Portal nav** (top bar; bottom tabs on mobile): Dashboard · My membership · My card · Benefits · Profile — plus settings via avatar menu (Preferences & privacy, Logout). A persistent status banner appears for `pending` (awaiting payment/approval) and `grace` states.

**Admin sidebar** (grouped, per 08 §5):

| Group | Items |
|-------|-------|
| Overview | Dashboard |
| People | Members |
| Partners | Flight schools · Associations · Aerodromes · Sponsors |
| Agreements | Contracts · Benefits |
| Comms | Campaigns · Templates · Send log |
| Fleet | Aircraft |
| System *(admin only)* | Users · Settings · Audit |

## 4. State-dependent access within the portal

A logged-in user's portal is shaped by member status (00 §7.2):

| Status | Portal behavior |
|--------|-----------------|
| *(no application yet)* | Only `/portal/apply` (and profile/settings); nav prompts application |
| `pending` | Dashboard shows awaiting payment/approval; `/portal/membership/pay` available; card page shows "not yet issued" |
| `active` | Everything |
| `grace` | Everything + warning banner; Renew is primary action everywhere |
| `expired` | Card renders expired (08 §6.3); benefits catalog locked with rejoin prompt; renewal available; history/profile/settings intact |
| `archived` | Login allowed; read-only history + GDPR self-service only |

## 5. RBAC access map

Matrix of role × area (roles locked in 00 §5). ✔ = full per PRD; **R** = read-only; — = no access (403).

| Area | visitor | member | staff | admin |
|------|---------|--------|-------|-------|
| Public pages, `/verify/{token}` | ✔ | ✔ | ✔ | ✔ |
| Auth pages | ✔ | ✔ (logout) | ✔ | ✔ |
| Portal (own data only) | — | ✔ | — * | — * |
| Admin dashboard & queues | — | — | ✔ | ✔ |
| Members management | — | — | ✔ | ✔ |
| Membership manual adjustment (ADM-009) | — | — | — | ✔ |
| Payment refund marking (ADM-006 AC3) | — | — | — | ✔ |
| Partners (schools/associations/aerodromes/sponsors) | — | — | ✔ | ✔ |
| Contracts & benefits | — | — | ✔ | ✔ |
| Campaigns, templates, send log | — | — | ✔ | ✔ |
| Fleet | — | — | ✔ | ✔ |
| Users (ADM-032) | — | — | — | ✔ |
| Club settings (ADM-033) | — | — | — | ✔ |
| Audit log (ADM-034) | — | — | **R** — no† | ✔ |
| GDPR erasure execution (ADM-035) | — | — | — | ✔ |

\* Staff/admin are not members in v1; `/portal/*` redirects them to `/admin`. † Staff have no audit access; audit is admin-only (00 §5).

Enforcement is dual-layer (PLT-002): the Next.js network boundary (`proxy.ts` in Next 16 — the renamed middleware) + server-action guards for routing, Supabase RLS as the data backstop (06 §RLS).

## 6. Redirects & error handling

| Situation | Behavior |
|-----------|----------|
| Anonymous → any `/portal/*` or `/admin/*` | Redirect to `/login?next={path}` |
| `member` → `/admin/*` | 403 page (PUB-014) |
| `staff`/`admin` → `/portal/*` | Redirect to `/admin` |
| Logged-in `active` member → `/join` | Redirect to `/portal` (PUB-009 AC2) |
| Unknown route | Localized 404 (PUB-014) |
| Unknown/revoked `{token}` | `/verify` renders invalid verdict, HTTP 200 (PUB-013 AC2) |
| Legacy/trailing-slash URLs | 301 to canonical form |

## 7. SEO & meta conventions

- Title pattern: `{Page} — Aeroskill Club`; home: `Aeroskill Club — {mission one-liner}`.
- `hreflang` pairs `ro` ↔ `en` on all public routes (PUB-011); canonical = self per locale.
- OG image: branded card-motif default; `sitemap.xml` lists public routes only; `/portal`, `/admin`, `/verify` are `noindex` (PUB-012).
- Structured data (JSON-LD): `Organization` (with `nonprofitStatus`) on `/`; `Offer` markup for the three tiers on `/membership` (price in RON, `priceValidUntil` tied to the 60-day-notice policy of 02 §7); `FAQPage` only if a real FAQ section ships. No review/rating markup — nothing to back it.
