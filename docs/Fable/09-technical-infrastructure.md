# 09 — Technical Infrastructure

> **Purpose:** stack, services, environments, security/GDPR posture, and deployment for the platform. Stack choices are locked in `00-foundation.md` §4.2 — this document operationalizes them. Depends on `04-prd.md` (PLT requirements) and `06-database-schema.md` (RLS, migrations).

---

## 1. Architecture overview

One Next.js app serving all three surfaces; Supabase as the data/auth/storage plane; a small set of external services.

```mermaid
flowchart TB
    subgraph Client
        PW[Public website] 
        MP[Member portal]
        AC[Admin CRM]
    end
    subgraph Vercel
        APP[Next.js 16 app<br/>SSR + Server Actions + proxy.ts]
        CRON[Vercel Cron → /api/cron/daily]
        WH[/api/webhooks/stripe]
    end
    subgraph Supabase
        AUTH[Auth]
        PG[(Postgres + RLS)]
        ST[Storage: logos, contracts, photos]
    end
    STRIPE[Stripe Checkout]
    RESEND[Resend email]
    PLAUS[Plausible analytics]

    PW & MP & AC --> APP
    APP --> AUTH
    APP --> PG
    APP --> ST
    APP --> STRIPE
    STRIPE -- signed webhook --> WH --> PG
    APP & CRON --> RESEND
    CRON --> PG
    PW & MP --> PLAUS
```

Key properties: no self-managed servers; the only stateful system is Supabase Postgres; every write path goes through Server Actions or the two machine endpoints (webhook, cron), all Zod-validated (PLT-008) and RLS-backstopped (PLT-002).

## 2. Stack detail

| Piece | Version / plan | Why for a solo Claude Code developer (00 §4.2) |
|-------|----------------|------------------------------------------------|
| Next.js | **16.x (LTS since 2025-10)**, App Router, TypeScript strict, Turbopack default | One framework, three surfaces via route groups; server-first by default; route gating in `proxy.ts` (Next 16's renamed middleware); React 19 + stable React Compiler |
| Supabase | Pro plan | Postgres + Auth + Storage + RLS in one managed service; local dev via `supabase` CLI |
| Tailwind CSS + shadcn/ui | 4.x / latest | Token-driven implementation of 08; components live in-repo, fully editable |
| next-intl | latest | Locale routing per 00 §4.4; typed message catalogs |
| Stripe | Checkout + webhooks | RON support; no card data ever touches the app (SAQ-A scope) |
| Resend + react-email | latest | Transactional + campaign batch sends from one provider; templates in the repo |
| Zod | latest | Single validation idiom at every boundary |
| Plausible | EU-hosted | Cookieless analytics (PLT-010) |
| Vitest + Playwright | latest | Test posture per 03 §philosophy |

## 3. Third-party services (doubles as GDPR processor list)

Plan prices verified against published pricing, July 2026:

| Service | Purpose | Cost / month (verified) | Personal data shared |
|---------|---------|-------------------------|----------------------|
| Vercel (Pro) | Hosting, cron, previews | ~100 RON ($20/seat) | IP addresses (transient logs) |
| Supabase (Pro) | DB, auth, storage, backups | ~125 RON ($25, includes $10 compute credit) | All member data (primary store, EU region) |
| Stripe | Card payments | 0 fixed; **≈1.5% + 1 RON** per EEA card, 100 RON per dispute | Name, email, payment metadata |
| Resend (Pro) | Email (transactional + campaigns) | ~100 RON ($20, 50k emails/mo — far above v1 volume) | Name, email addresses |
| Plausible | Analytics | ~45 RON (€9, 10k pageviews/mo tier) | None (aggregate, cookieless) |
| Domain + misc | DNS | ~10 RON | — |
| **Total fixed** | | **~480 RON/month** | |

Card-fee context: at the Year-1 base case (435,000 RON dues, 02 §5) an all-card year costs ~6,700 RON in Stripe fees (~1.5%); every bank-transfer payment reduces that but adds staff reconciliation minutes (FLOW-10). Netopia plan B at 0.99% + 0.30 RON would roughly halve card fees at the cost of a heavier integration (00 §4.3) — revisit only if Stripe onboarding fails or card volume makes the delta material (>500 members).

All providers under EU SCCs or EU-region hosting; DPAs signed with each (GDPR non-negotiable, 00 §8.1). This table is the processor list referenced by the privacy policy (PUB-010 AC2).

## 4. Environments

| Env | Purpose | Data | URL |
|-----|---------|------|-----|
| **Local** | Development | `supabase start` local stack; seeded demo data (06 §6) | localhost |
| **Preview** | Per-PR review | Isolated Supabase preview branch or shared staging project; demo data only, **never** production data | `*.vercel.app` per PR |
| **Production** | Live | Real data; EU region (Frankfurt) | `aeroskill.club`* |

\* Domain assumed; confirm before launch (10 §dependencies). Stripe and Resend run in test mode everywhere except production.

## 5. Configuration & secrets

Environment-variable registry (all secrets in Vercel/Supabase env config, never in the repo — 00 §8.2):

| Variable | Purpose | Envs |
|----------|---------|------|
| `NEXT_PUBLIC_SITE_URL` | Canonical origin (QR URLs, emails) | all |
| `NEXT_PUBLIC_SUPABASE_URL` / `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Client SDK | all |
| `SUPABASE_SERVICE_ROLE_KEY` | Server-only privileged ops (webhook, cron, erasure) | all (server) |
| `SUPABASE_DB_URL` | Migrations in CI | CI |
| `STRIPE_SECRET_KEY` / `STRIPE_WEBHOOK_SECRET` | Checkout + webhook verification | all |
| `RESEND_API_KEY` | Email | all |
| `CRON_SECRET` | Authenticates `/api/cron/daily` (05 §2) | all |
| `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` | Analytics | prod |

Rule: any new variable is added to this table in the same PR that introduces it (03 §spec-update rule).

## 6. Security

- **Authentication:** Supabase Auth, HTTP-only secure cookies, email confirmation mandatory (PLT-001); password ≥ 10 chars + breach check (MEM-001).
- **Authorization:** three enforcement points — the Next.js network boundary `proxy.ts` (route gating per 05 §5), server-action guards (role re-check per action), **RLS as the backstop** (06 §5, role via JWT claim from the Supabase auth hook). UI bugs cannot exceed data rights. Role demotion/deactivation revokes sessions server-side (06 §5.3).
- **Webhooks:** Stripe signature verification + `stripe_events` idempotency ledger (PLT-009). Cron endpoint requires `CRON_SECRET` header.
- **Rate limiting:** per-IP on `/login`, `/reset-password`, `/contact`, `/verify/{token}` (PLT-011) enforced in the `proxy.ts` boundary, with an in-memory fallback acceptable at v1 scale (04 §5 data volumes).
- **Input validation:** Zod at every boundary (PLT-008); file uploads type- and size-checked (MEM-010, ADM-019); storage buckets private by default — public bucket only for sponsor logos and aircraft photos.
- **Secrets:** env-only, least privilege: the browser only ever sees the anon key; the service-role key is server-side only.
- **Backups:** Supabase daily automated backups, 30-day retention; **restore drill before public launch** (04 §5, 10 §launch checklist).
- **Incident basics:** provider status pages monitored; single-page runbook in the repo covering: revoke keys, restore DB, pause Stripe, status note on the public site.

## 7. GDPR

**Data map** (category → purpose → legal basis → retention):

Supervisory authority: **ANSPDCP** (Autoritatea Națională de Supraveghere a Prelucrării Datelor cu Caracter Personal).

| Data | Purpose | Legal basis | Retention |
|------|---------|-------------|-----------|
| Identity & contact (name, email, phone, county, DOB) | Membership administration + statutory member register (OG 26/2000, 00 §2) | Contract (membership terms) + legal obligation (register) | Membership + 3 years |
| Pilot status | Tier/benefit relevance | Legitimate interest | As above |
| Payment records | Dues accounting | Legal obligation | **5 years** for supporting documents per Law 36/2023 (from July 1 of the year after the financial year; annual financial statements — kept by the accountant, not the platform — 10 years); anonymized on erasure |
| Marketing consent + campaign sends | Communication | Consent (opt-in, MEM-019) | Until withdrawn |
| Card verification hits | Fraud prevention | Legitimate interest | Aggregate only, no PII (PLT-010) |
| Audit logs | Security/accountability | Legitimate interest | 3 years |
| Rejected applications | Processing the application | Pre-contract steps | Purged at +90 days (ADM-005) |

**Subject rights implementation:** access/portability → self-service JSON export (MEM-021); erasure → request + admin execution with anonymize-in-place, auth deletion, payment anonymization (MEM-022/ADM-035, FLOW-08); rectification → profile edit (MEM-009) or staff correction (ADM-007); consent withdrawal → one click in `/portal/settings` or any marketing email's unsubscribe link.

**Cookies:** auth session cookie (strictly necessary) + zero third-party cookies (Plausible is cookieless) → cookie notice, no consent banner (00 §4.5).

**Accessibility law (researched):** the European Accessibility Act (enforced 2025-06-28) covers e-commerce services; the club almost certainly qualifies for the services **microenterprise exemption** (<10 employees *and* <€2M turnover — both conditions required, and the exemption lapses immediately if either threshold is crossed). Policy: build to **WCAG 2.2 AA** regardless (00 §8.3, 08 §7) and publish the accessibility statement (PUB-015); harmonized-standard status: EN 301 549 v3.2.1 (WCAG 2.1 AA) today, v4.1.1 (WCAG 2.2) expected 2026.

**Fiscal boundary (researched):** RO e-Factura is mandatory B2C since 2025-01-01 and for NGOs with economic activity since 2025-07-01. Membership *cotizații* are dues, not invoiceable supplies — outside e-Factura. **Sponsorship/service invoices to companies are B2B e-Factura documents, issued by the club's accountant via ANAF SPV, entirely outside this platform** (00 §2). The platform stores contract values and payment confirmations only; the accountant's SPV workflow must be confirmed before the first sponsor contract (10 §3).

## 8. Deployment & CI

Pipeline (GitHub Actions + Vercel):

1. PR opened → lint (`eslint`, `tsc --noEmit`), unit tests (Vitest), build; Vercel preview deploy.
2. Migration check: `supabase db diff` must be empty against committed migrations (schema drift fails CI).
3. Merge to `main` → production deploy; migrations applied via `supabase db push` in a pre-deploy step, in the 06 §6 order; Playwright smoke (golden path FLOW-01 in test mode) against preview before promote.
4. Rollback: Vercel instant rollback for app code; migrations are forward-only — a bad migration ships a corrective successor (no down-migrations in production).

**Scheduled jobs:** Vercel Cron hits `/api/cron/daily` at 06:00 Europe/Bucharest → status transitions (PLT-006), reminder emails, expiry alerts (PLT-005). Idempotent: re-running a day is safe (transition conditions are state-based, reminders keyed by `email_log` dedupe).

## 9. Observability

- **Error tracking:** Vercel runtime logs + a lightweight Sentry (free tier) integration for server-action and webhook exceptions.
- **Uptime:** external ping on `/` and `/verify/health-token-check` every 5 minutes (UptimeRobot free) — the verification page is the availability-critical path (04 §5).
- **The five alerts that matter:** production error spike; webhook failures (Stripe event unprocessed > 1 h); cron job missed/failed; Supabase storage/DB capacity > 80%; uptime check down > 5 min.
- **Business observability** lives in the CRM dashboard (ADM-001), not in ops tooling.

---

*Sources for the researched facts in this document: [Next.js 16 release](https://nextjs.org/blog/next-16), [Vercel pricing](https://vercel.com/pricing), [Supabase pricing](https://supabase.com/pricing), [Resend pricing](https://resend.com/pricing), [Plausible plans](https://plausible.io/docs/subscription-plans), [Stripe Romania pricing](https://stripe.com/en-ro/pricing), [Netopia fees review](https://noda.live/ro/articles/recenzie-netopia-payments), [Law 36/2023 retention](https://www.contzilla.ro/documentele-contabile-se-vor-pastra-in-arhiva-contabila-5-ani-in-loc-de-10-ani/), [e-Factura NGO deadlines](https://www.avocatnet.ro/articol_67338/e-Factura-ONG-urile-cultele-%C8%99i-agricultorii-persoane-fizice-scap%C4%83-temporar-de-obliga%C8%9Bia-folosirii-sistemului.html), [EAA requirements](https://accessible.org/eaa-ecommerce-services-requirements/), [Supabase RLS guidance](https://supabase.com/docs/guides/troubleshooting/rls-performance-and-best-practices-Z5Jjwv). Full research basis: 00 §10.*
