# Aeroskill Club — Planning Documentation

A complete planning set for **Aeroskill Club**, a modern, bilingual (Romanian-first / English-second) **general-aviation members' club** platform for the Romanian GA market, designed to be built by **one developer using Claude Code**. This is a **concept / portfolio** body of work: architecturally and visually rigorous, grounded in real Romanian/EASA aviation context, but not bound to hard production legal/tax/payment compliance (those are flagged, not solved).

The platform spans three surfaces:
- **Public website** — mission, three membership tiers, sponsors, join funnel.
- **Member area** — profile, aviation profile, subscription, payment, digital member card, benefits, events.
- **Admin CRM** — members, partner organizations (flight schools, associations, aerodromes, sponsors), benefits, contracts, communications, fleet.

---

## How to read this set

Start with **`00-foundation.md`** — it is the locked source of truth that every other document inherits from (positioning, the three tiers and their prices, the entity glossary, naming conventions, the technology stack, and the logo-driven brand tokens). Then read in numeric order, or jump to what you need.

| # | Document | What it answers |
|---|----------|-----------------|
| 0 | [00-foundation.md](Opus/00-foundation.md) | The canonical decisions everything else inherits — read first |
| 1 | [01-product-vision.md](Opus/01-product-vision.md) | Why we exist, who we serve, what success looks like |
| 2 | [02-product-strategy.md](Opus/02-product-strategy.md) | Positioning, tier & sponsor strategy, monetization, GTM, risks |
| 3 | [03-implementation-plan.md](Opus/03-implementation-plan.md) | Build philosophy, scope slices, milestones, solo Claude Code workflow |
| 4 | [04-prd.md](Opus/04-prd.md) | Detailed requirements per surface, with IDs & acceptance criteria |
| 5 | [05-information-architecture.md](Opus/05-information-architecture.md) | Sitemap, navigation, URL/i18n structure, RBAC access map |
| 6 | [06-database-schema.md](Opus/06-database-schema.md) | Entities, columns, relationships, ERD, RLS posture |
| 7 | [07-user-flows.md](Opus/07-user-flows.md) | Step-by-step flows across all three surfaces |
| 8 | [08-design-system.md](Opus/08-design-system.md) | Logo-driven brand, tokens, type, components, member card |
| 9 | [09-technical-infrastructure.md](Opus/09-technical-infrastructure.md) | Stack, services, environments, security/GDPR, deployment |
| 10 | [10-roadmap.md](Opus/10-roadmap.md) | Sequenced solo Claude Code build roadmap, phase by phase |

**Suggested reading paths**
- *Product / strategy reviewer:* 00 → 01 → 02 → 10
- *Engineer about to build:* 00 → 04 → 06 → 09 → 03 → 10
- *Designer:* 00 → 08 → 05 → 07
- *Whole picture:* 00 through 10 in order

---

## Locked decisions at a glance

| Aspect | Decision |
|---|---|
| **Market** | Romania, within the EU / EASA framework (AACR national CAA; ULM via SAUM) |
| **Languages** | Romanian-first, English-second (bilingual, i18n from day one) |
| **Tiers** | **Cadet** (free) · **Aviator** (490 RON/yr, "Most popular") · **Captain / Comandant** (1.490 RON/yr; Founding/Life 4.990 RON) |
| **Currency** | RON primary, EUR shown secondary |
| **Stack** | Next.js 15 · next-intl · Tailwind + shadcn/ui · Supabase (Postgres+Storage+RLS, EU) · Supabase Auth · Stripe Billing · Resend · Vercel |
| **Member card** | Web/PDF + QR → Google Wallet → Apple Wallet (phased) |
| **CRM data model** | Hybrid **Party model** (person/organization + roles) |
| **Brand** | Deep navy `#102844` (from the logo) · Space Grotesk + Inter + IBM Plex Mono · light public theme + dark "cockpit" member/admin theme |
| **Legal form** | Non-profit *asociație* (OG 26/2000); concept-grade compliance |

The brand is driven by the real logo in [`../logo/`](../logo) — an uppercase geometric-grotesk **"AEROSKILL ✈ CLUB"** wordmark with a climbing aerobatic low-wing monoplane icon, in deep navy.

---

## How this set was produced

These documents were grounded in a multi-stream research pass (Romanian/EASA regulatory landscape, aviation membership models, member portals & digital cards, payments/GDPR for a Romanian org, solo-developer stack, CRM/fleet data modeling, and brand/bilingual UX), then drafted from the shared `00-foundation.md` and reconciled through a consistency, cross-reference, and Romanian/EASA factual-accuracy review pass.

**Status:** Concept draft, 2026-06-28. Specific figures (prices, license hours, validity windows, vendor pricing, market sizing) are point-in-time and **illustrative** — re-verify before any public, legal, or financial use.
