# Aeroskill Club — Canonical Foundation

> **Status:** Locked source of truth · **Date:** 2026-06-28 · **Scope:** Concept / portfolio build
> Every other document in `docs/` (01–10) inherits the vocabulary, decisions, and conventions defined here. If a later document contradicts this file, this file wins until explicitly amended. Amend here first, then propagate.

---

## 0. How to use this document

This is the **shared brain** for the Aeroskill Club platform planning set. It pins down the things that must be identical across all ten documents: the positioning, the three membership tiers and their prices, the entity/data vocabulary, the Romanian/EASA domain terms, the naming conventions, the technology stack, and the brand tokens. Read this first; then read the document you need.

The ten downstream documents are:

| # | File | Owns |
|---|------|------|
| 1 | `01-product-vision.md` | Why we exist, who we serve, what success looks like |
| 2 | `02-product-strategy.md` | Positioning, membership & sponsor strategy, monetization, GTM, risks |
| 3 | `03-implementation-plan.md` | Build philosophy, scope slices, milestones, solo workflow with Claude Code |
| 4 | `04-prd.md` | Detailed requirements per surface, with acceptance criteria |
| 5 | `05-information-architecture.md` | Sitemap, navigation, content model, URL/i18n structure, RBAC map |
| 6 | `06-database-schema.md` | Entities, columns, relationships, ERD, RLS posture |
| 7 | `07-user-flows.md` | Step-by-step flows across all three surfaces |
| 8 | `08-design-system.md` | Brand direction, tokens, type, components, motifs (logo-driven) |
| 9 | `09-technical-infrastructure.md` | Stack, services, environments, security/GDPR, integrations, deploy |
| 10 | `10-roadmap.md` | Sequenced solo Claude Code build roadmap with effort & dependencies |

---

## 1. The product in one paragraph

**Aeroskill Club** is a modern, bilingual (Romanian-first, English-second) **general-aviation members' club** based in Romania. It is a digital platform with three surfaces: a **public marketing website** (mission, member benefits across three tiers, sponsors), a **member area** (profile, subscription, payment, digital member card, benefits, events), and an **admin CRM** (members, partner organizations — flight schools, associations, aerodromes, sponsors — benefits, contracts, communications, and fleet). It is built to be deliverable by **one developer using Claude Code**, and it is a **concept / portfolio piece**: architecturally and visually rigorous, grounded in real Romanian/EASA aviation context, but not bound to hard production compliance (final legal/tax/payment contracts are out of scope and flagged where relevant).

---

## 2. Audience & personas

Primary market: the **Bucharest-area and national Romanian GA scene**, plus international/visiting pilots (hence bilingual).

| Persona | Who | What they want | Likely tier |
|---|---|---|---|
| **Andrei – the aspiring pilot** | 19, student, eyeing a PPL/LAPL or ULM permit | Community, cheaper training, "am I cut out for this?" | Cadet → Aviator |
| **Ioana – the active private pilot** | 38, PPL(A) with SEP + Night, flies ~40h/yr | Fuel/landing discounts, rating-expiry reminders, networking, events | Aviator |
| **Mihai – the owner/instructor** | 52, owns a YR- aircraft, FI, semi-pro | Insurance/legal advocacy, priority everything, recognition, concierge | Captain |
| **Elena – the enthusiast** | 45, non-pilot aviation fan, photographer | Events, fly-ins, community, supporting the scene | Cadet |
| **A partner org** | Flight school / aerodrome / sponsor | Reach engaged pilots, manage the agreement, track redemptions | (CRM-side) |

Disciplines are a **multi-select**, not airplane-only: airplane (LAPL/PPL), glider/sailplane (SPL), balloon (BPL), ultralight/ULM, parachuting, plus "enthusiast / non-pilot".

---

## 3. Positioning & differentiation

- Aeroskill Club is a **benefits + community + advocacy layer on top of** the existing Romanian GA infrastructure — **not** a flight school, **not** an ATO, and **not** a competitor to the state **Aeroclubul României**. It partners with schools, aeroclubs, and aerodromes.
- The incumbent national body (Aeroclubul României) is **Romanian-only, institutional, and visually dated**. The whitespace Aeroskill owns is **modern + bilingual + premium-yet-approachable**.
- Local credibility anchor: **ACR (Automobil Clubul Român)** — Romanians already pay ~200–250 RON/yr for a serious club with a card, assistance, and reciprocity. Aeroskill is "ACR for pilots", done in a modern way.
- Differentiation pillars: (1) **bilingual, modern UX**; (2) a **real digital member card** with partner-redeemable benefits; (3) **advocacy & community** (aspirational IAOPA-style affiliation path); (4) **deep partner network** (schools, aerodromes, sponsors) surfaced as tangible perks.

---

## 4. Membership model (LOCKED)

Three tiers, "good / better / best", with a free entry tier for funnel volume, a flagged middle anchor, and a premium top tier. **RON is the primary currency; EUR shown secondary.** Prices are concept figures calibrated against ACR (~250 RON/yr), Romanian net wages (~5.674 RON/mo, Q4 2025), and gym benchmarks.

| | **Cadet** | **Aviator** *(Most popular / Cel mai popular)* | **Captain** |
|---|---|---|---|
| RO tier name | Cadet | Aviator | Comandant |
| EN tier name | Cadet | Aviator | Captain |
| Price | **Free** (0 RON) | **490 RON/yr** (~€99) · or 49 RON/mo | **1.490 RON/yr** (~€299) · or 149 RON/mo |
| One-time option | — | — | **Founding / Life: 4.990 RON** (~€999) |
| Target | Students, youth (16–23), enthusiasts, prospective pilots | Active LAPL/PPL pilots, students in training | Owners, instructors, professionals, patrons, corporate |
| Headline value | Digital member card, bilingual community + events access, club newsletter, basic partner discounts, advocacy voice | Everything in Cadet **+** meaningful fuel/landing & training discounts, priority event seating, member directory, rating-expiry reminders, optional **Family add-on** (capped) | Everything in Aviator **+** negotiated insurance/legal advocacy layer, concierge/priority helpline, **premium physical metal card**, website recognition, deepest discounts (conditioned on recurrent-training proof) |
| Tier accent | Sky | Brass | Engraved navy + brass |

**Design principles (locked):** one **shared benefit core** across all tiers (community, card, advocacy); tiers differ by **depth of service** (discount size, protection, concierge), never by withholding basic access. Middle tier visually flagged "Most popular". Top tier is consciously a price **anchor**. Add-ons are variants, not separate ladders: **Family** (capped, ADAC-style) on Aviator+; **Founding/Life** one-time on Captain.

Membership **dues are always separate** from any flying/training spend. If fleet/flight access is ever shown, it is a distinct wet/dry hourly line item, never bundled into tier price.

---

## 5. Surfaces & roles

**Three surfaces:**
1. **Public site** — marketing, mission, tiers/pricing, sponsors, join funnel, content (RO/EN). Default theme: **light**.
2. **Member area** — authenticated self-service portal. Default theme: **dark "cockpit"**.
3. **Admin CRM** — staff back-office. Theme: dark "cockpit".

**Roles (RBAC):**
- `visitor` — unauthenticated.
- `member` — authenticated; effective permissions scoped by **tier** (cadet / aviator / captain).
- `staff` — CRM access (configurable per-module permissions).
- `admin` — full CRM + settings + user management.

(Auth via Supabase Auth; `auth.uid()` drives Row Level Security — see §11.)

---

## 6. Feature inventory (master list)

This is the canonical feature set. The PRD (04) expands each into requirements; the roadmap (10) sequences them. **MoSCoW** tags scope for the concept build: **[M]** must, **[S]** should, **[C]** could (later phase).

### Public site
- Home / hero + value proposition **[M]**
- Mission & about, the club story **[M]**
- Membership tiers & pricing comparison table **[M]**
- Benefits catalog (public preview) **[S]**
- Sponsors / partners showcase **[M]**
- Join / sign-up entry → checkout **[M]**
- Content/news (MDX) **[S]**
- Contact + legal (privacy, terms, GDPR notice) **[M]**
- Bilingual RO/EN with `/ro` `/en` routing + hreflang **[M]**

### Member area
- Auth: sign-up, login, email verify, password reset **[M]**
- Profile: personal details, person/organization type **[M]**
- **Aviation profile:** licenses, ratings, medical certificate, home aerodrome, disciplines **[M]**
- Subscription: current tier, upgrade/downgrade, renew, cancel (Stripe Customer Portal) **[M]**
- Payment: Stripe Checkout, payment history, invoices/receipts **[M]**
- **Digital member card:** web/PDF card + QR; Google Wallet **[S]**; Apple Wallet **[C]**
- Benefits: catalog + eligibility by tier + redemption **[S]**
- Events: list + RSVP **[S]**
- Member directory (opt-in; Aviator + Captain) **[S]**
- Documents vault: licenses/medicals/receipts **[S]**
- Communication preferences + consent center; data export/erasure **[M]**
- Rating/medical/expiry reminders **[S]**

### Admin CRM
- Dashboard: KPIs + **compliance/expiry** widget **[S]**
- Members management (the Party model, member role) **[M]**
- Partner organizations: flight schools/ATOs, associations/aeroclubs, aerodromes, sponsors/vendors, CAMO/CAO **[M]**
- Contracts/agreements + documents + renewal tracking **[M]**
- Benefits catalog management + tier eligibility + redemptions ledger **[S]**
- Communications: activity log, campaigns, segments, **consent ledger** **[S]**
- Fleet: aircraft + airworthiness/ARC + insurance + maintenance + hours + bookings **[S]**
- Subscriptions & payments overview **[S]**
- Reference data: regulators/authorities, aerodromes, license/rating vocabularies **[M]**
- Settings, roles & permissions, audit log **[S]**

---

## 7. Canonical entity glossary (data vocabulary)

The CRM is built on a **hybrid Party model**. These names are authoritative; the DB schema (06) implements them as tables (plural snake_case — see §9).

**Core / Party**
- **Party** — anyone we interact with; discriminator `party_kind` ∈ {person, organization}.
- **PersonProfile** / **OrganizationProfile** — 1:1 subtype tables off Party.
- **PartyRole** — a capacity a party acts in; `role` ∈ {member, flight_school_ato, partner_association, aerodrome, sponsor_vendor, camo_cao, regulator}. A party may hold several.
- **PartyRelationship** — typed edge between two parties (e.g., aircraft owner → aerodrome; school → aerodrome).
- **ContactChannel**, **Address** — reusable, attached to any party.

**Membership domain**
- **Member** — a Party with the `member` role (usually a person; orgs allowed for corporate).
- **MembershipTier** — Cadet / Aviator / Captain (config-driven; links to Stripe Price).
- **Membership** (a.k.a. Subscription) — a member's enrollment in a tier; status, period, Stripe subscription ref.
- **MemberCard** — issued card record: tier, member number, holder name, status, issued/expires, serial, QR token.
- **AviationProfile** child records on a member: **License** (type, authority, number, expiry), **Rating** (SEP/MEP/Night/IR…, validity window, expiry), **MedicalCertificate** (class 1/2/LAPL, expiry, examiner).

**Benefits domain**
- **Benefit** — catalog item (bilingual name/description, category, `providing_party_id`).
- **BenefitTierEligibility** — join (benefit × tier).
- **Redemption** — ledger row (member, benefit, code, issued/redeemed, value, location, status).

**Partners & agreements**
- **Contract** — agreement with a partner party; status, dates, value/currency, auto-renew, `renews_from_contract_id`.
- **ContractDocument** — signed PDF(s) per contract.

**Communications domain**
- **Activity / Interaction** — CRM timeline entry (call/email/meeting/note).
- **Campaign** + **CampaignRecipient** — bulk sends.
- **Segment** — saved filter criteria.
- **Consent** — GDPR consent ledger (party, purpose, status, lawful_basis, source, channel, timestamp, version).

**Fleet domain**
- **Aircraft** — natural key = registration (YR-xxx); icao_type_designator, manufacturer, model, category/class, seats, MTOW, owner, home aerodrome, status.
- **AircraftAirworthiness** — CofA, ARC issue/expiry, extension_count, managing CAMO/CAO.
- **AircraftInsurance** — policy, insurer, coverage, expiry.
- **MaintenanceLog** — date, type, description, next-due (hours/date).
- **FlightLog / HoursEntry** — hobbs/tach, airframe total.
- **Booking** — reservation against aircraft (+optional instructor); no-overlap per aircraft; gated by eligibility (valid medical/rating/paid membership).

**Billing & docs**
- **Payment** — Stripe-driven; stores Stripe IDs + brand/last4 only.
- **Invoice** — e-Factura-ready fields; separates "membership-fee receipt" from "commercial invoice".
- **Attachment / Document** — polymorphic (owner_type + owner_id) for contracts, member docs, aircraft docs.

**Events & platform**
- **Event** + **EventRSVP**.
- **User** (auth) ↔ Party; **Role/Permission**; **AuditLog**.
- **ReferenceData** lookups: authorities (AACR, ROMATSA, EASA, SAUM), license types, rating types, aircraft categories, aerodromes.

**Cross-cutting conventions:** soft-delete (`deleted_at`) on members/partners/contracts/aircraft; audit columns (`created_at/by`, `updated_at/by`) on every table; bilingual `name_ro` / `name_en` on parties, benefits, tiers, events. Avoid EAV — fixed subtype tables + enums + a single optional `extra_attributes jsonb` per major entity.

---

## 8. Romanian / EASA domain glossary & seed data

Use these **real** terms and entities so the concept reads as credible. Treat exact figures/codes as illustrative (verify before any public claim).

**Authorities & bodies**
- **AACR** — Autoritatea Aeronautică Civilă Română (national CAA, est. 1993, caa.ro). Issues EASA Part-FCL licenses; oversees personnel/aircraft/aerodromes/ATOs. The canonical "regulator".
- **EASA** — sets the EU framework (Part-FCL, Part-MED, Part-21/Part-ML). Does **not** issue licenses directly.
- **ROMATSA** — sole Romanian air navigation service provider (reference entity only).
- **SAUM** — Serviciul Aeronave Ultraușoare Motorizate, **inside Aeroclubul României**, accredited by AACR. **Issues ULM licenses/airworthiness — NOT AACR.** Critical EASA-vs-national distinction.
- **Aeroclubul României** — state national aeroclub (founded 1920, FAI-affiliated 1923); ~15–16 territorial aeroclubs; trains PPL(A)/ULM/glider/parachute; free youth courses (~16–23).

**Licenses (model TYPE separate from RATINGS):** LAPL(A) (~30h, VFR, ≤2000 kg), PPL(A) (~45h), CPL/ATPL, **SPL** (glider, Part-SFCL), **BPL** (balloon, Part-BFCL), **ULM permit** (national, issued by SAUM).
**Ratings (own validity/expiry each):** SEP (24-mo validity), MEP, **Night**, **IR** (12-mo). Validity windows are **data-driven**, not hard-coded.
**Medicals (hierarchy):** Class 1 (AeMC; CPL/ATPL) ⊃ Class 2 (AME; PPL/SPL/BPL) ⊃ LAPL medical (AME/GP). Model "highest medical held" + expiry.
**"Current to fly"** = valid license **AND** valid rating **AND** valid medical (computed, not a single flag).

**Aircraft categories (enum):** CS-23 certified · ELA/LSA (≤600 kg) · experimental/amateur-built · **national ULM** (450 / 472.5 with ballistic chute / 600 kg microlight). Romanian registration prefix **YR-**.
**Airworthiness:** non-expiring CofA validated by **ARC** (EASA Form 15c, 1-yr, extendable max ×2 by a CAMO/CAO). Track as a recurring expiry.

**Real seed entities (for CRM demo data):**
- *Flight schools / ATOs:* Aeroclubul României (15 territorial sites), Regional Air Services (Tuzla, LRTZ; first private RO ATO, approval RO/ATO-06), Romanian Aviation Academy / SSAvC (Strejnic/Băneasa, A320 FFS), Jetav, Aerowest (Timișoara/Buziaș), Transylvania Wings (Brașov-Ghimbav). Distinguish **ATO** (approved) vs **DTO** (declared).
- *Associations:* AOPA Romania (aopa.ro, IAOPA-affiliated), BGAA (bgaa.ro), Aeroas, OPIAR.
- *Aerodromes:* Clinceni (LRCN), Strejnic/Ploiești (LRPV), Tuzla (LRTZ, private), Băneasa, Iași-Sud, Deva, Craiova-Podari, Cluj-Dezmir.
- *Territorial aeroclub cities:* București (Clinceni), Ploiești, Pitești, Brașov, Iași, Suceava, Deva, Sibiu, Târgu Mureș, Cluj-Napoca, Baia Mare, Satu Mare, Oradea, Arad, Craiova, Caransebeș.

**Legal form:** the club is naturally a non-profit **asociație** under **OG 26/2000** (≥3 founders, statute, court registration; members may be persons (CNP) or legal entities (CUI)). Membership fees (*cotizații*) are non-taxable; selling sponsorships/paid services is economic activity (may trigger VAT/e-Factura).

---

## 9. Naming & data conventions

- **Database:** PostgreSQL (Supabase). Tables **plural snake_case** (`members`, `contracts`, `aircraft`). Columns snake_case. PK `id uuid default gen_random_uuid()`. Timestamps `timestamptz`. FKs `<entity>_id`.
- **Enums:** Postgres enums or `text` + `check` for controlled vocabularies (role, status, tier, license_type, rating_type, aircraft_category, consent_purpose).
- **Bilingual columns:** `*_ro` / `*_en` (e.g., `name_ro`, `name_en`, `description_ro`, `description_en`).
- **Money:** store `amount_minor int` (bani) + `currency char(3)` (default `RON`). Never assume one currency.
- **Soft delete + audit:** `deleted_at`, `created_at/by`, `updated_at/by` everywhere relevant.
- **Money/dates display:** always via `Intl` with explicit `ro-RO` / `en-GB` locales. RO: `1.234,56 lei`, `28.06.2026`, 24h. EN: secondary.
- **Code:** TypeScript, App Router conventions, `kebab-case` files/routes, `PascalCase` components. **No hardcoded UI copy** — all strings in `next-intl` message catalogs (`ro.json`, `en.json`) from day one, across all three surfaces.
- **URLs:** locale path-prefix `/ro/...` `/en/...`; RO default; reciprocal `hreflang` (`ro-RO`, `en`, `x-default`).

---

## 10. Technology stack (LOCKED)

One coherent, low-ops, single-developer stack:

| Concern | Choice | Notes |
|---|---|---|
| Framework | **Next.js 15** (App Router, TypeScript) | SSR/RSC, one repo for all 3 surfaces |
| i18n | **next-intl** | `[locale]` segment, RO default / EN peer |
| UI | **Tailwind CSS + shadcn/ui + Radix** | one design system across site/member/admin |
| Database + storage + RLS | **Supabase** (Postgres), **EU / Frankfurt (eu-central-1)** | single project; Storage for card assets/docs |
| Auth | **Supabase Auth** *(primary; Better Auth = documented alternative)* | `auth.uid()` integrates with RLS; simplest for solo |
| Payments | **Stripe** (Billing + hosted Checkout + Customer Portal + Stripe Tax) | RON-primary prices; 19% VAT/OSS; SAQ-A PCI scope |
| Local payment (future) | **Netopia mobilPay** | RON-native fallback, later phase |
| Email | **Resend + react-email** (EU sending region) | welcome, receipts, renewal/expiry reminders |
| Member card | Web/PDF + QR → **Google Wallet** (free) → **Apple Wallet** (.pkpass, $99/yr) | phased; `passkit-generator` for Apple |
| Marketing content | **MDX in-repo** → **Payload CMS 3** later | no separate CMS for the concept |
| Hosting | **Vercel** (concept) | **Coolify on Hetzner (EU)** = documented migration path |
| Tooling | TypeScript, ESLint/Biome, Zod, React Hook Form, TanStack Table | |

**Data residency note:** Supabase/Stripe/Resend/Vercel offer EU regions but are US-incorporated (CLOUD-Act exposure). Acceptable for a concept; the privacy notice must disclose it because we model sensitive pilot data (license numbers, medical class).

---

## 11. Security, privacy & compliance posture (concept-level)

- **GDPR** (RO Law 190/2018, ANSPDCP): lawful basis = **contract performance** for core membership; **separate, granular, withdrawable consent** for marketing/sponsor sharing. Consent ledger in the schema. Member-facing **privacy center** (export/erasure). Avoid biometric data on the card.
- **PCI:** hosted **Stripe Checkout** → SAQ-A. **Never store card data** — only Stripe IDs + brand/last4.
- **RLS:** members read/write only their own rows; staff/admin per-role. Enforced in Postgres, not just the app.
- **Invoicing:** Invoice entity is **e-Factura-ready** (issuer/buyer fiscal IDs, line items, VAT rate, submission status) even if v1 issues simple *cotizație* receipts. Membership-fee documents kept distinct from commercial invoices.
- **VAT:** Romania 19%; Stripe Tax computes/OSS-reports but the club is merchant of record (a real legal/accounting decision — flagged, not solved here).
- All of the above are **concept-grade**; real legal/tax/payment review is explicitly out of scope.

---

## 12. Brand foundations (logo-driven)

The real logo lives at `logo/` (`logo-full-transparent.webp`, `aeroskill club logo and text.jpg`, `aeroskill club logo only.jpg`): an uppercase geometric-grotesk **"AEROSKILL ✈ CLUB"** wordmark split by a dynamic climbing **aerobatic single-prop low-wing monoplane** silhouette, monochrome **deep navy**. The icon-only mark is a strong standalone silhouette (favicon / app icon / member-card watermark).

**Color tokens (re-skinnable; keep brand colors in one tokens file):**
- `--brand-navy: #102844` (sampled from logo; confirm against vector — may read slightly brighter ~#1B2A4A). Darkest surface `--navy-900 #0A1A33`, `--navy-800 #0F2440`, `--navy-700 #1B3A63`.
- Light: `--paper #F7F9FC`, `--ink #0A1322`; on-dark text `--cloud #E8EDF4`.
- Sky accent (secondary/links/CTA): `--sky-500 #2E7DD1`, `--sky-400 #4F9BE8`, `--sky-100 #DCEBFB`.
- Warm metal (premium, large/decorative only): `--brass-500 #B6883E`, `--brass-300 #D9B26A`.
- Signal (cockpit convention): `--ok-green #2FA66B`, `--caution-amber #E0A106`, `--warn-red #D14848`.
- Tier accents: Cadet = sky · Aviator = brass · Captain = engraved navy+brass.

**Type (free, self-hostable):** Display = **Space Grotesk** (echoes the wordmark); Body/UI = **Inter**; Technical mono = **IBM Plex Mono** (tail numbers, member IDs, coords, dates). Verify Space Grotesk renders true comma-below **ș/ț** + ă/â/î; fall back to Inter for RO display strings if a glyph fails.

**Themes:** Public = **light** (airy, approachable). Member + Admin = **dark "cockpit"** (focused, premium). Every color maps to a semantic token (`--bg`, `--surface`, `--text`, `--text-muted`, `--accent`, `--border`, `--focus`) so themes + a future logo swap are one token file.

**Motif language:** restrained "instrument + horizon" kit — attitude-indicator divider lines, compass/heading ticks as section markers, runway numbers & contrail strokes as sparing accents, the aircraft silhouette as a brand stamp. Line icons, 2px stroke. Real GA photography over stock sunsets. Subtle, instrument-like motion.

**Voice:** confident, precise, warm — "an experienced captain briefing a friend". Correct aviation vocabulary (PPL(A), LAPL, ratings, aerodrome) signals credibility; plain bilingual language keeps it welcoming. Light aviation microcopy ("Cleared for takeoff" on signup) used sparingly. RO copy native-written, never machine-translated.

**Accessibility:** WCAG 2.2 AA — 4.5:1 body, 3:1 large/UI/focus. Brass restricted to large/decorative. Visible 3:1 focus ring. Never color-only meaning (pair with icon/label).

---

## 13. Open decisions & assumptions log

These are reasoned defaults made for the concept; flagged so the user can override.

1. **Tier names** Cadet / Aviator / Captain (RO: Cadet / Aviator / Comandant) — assumed, not user-specified.
2. **Prices** (0 / 490 / 1.490 RON·yr; 4.990 Founding) — concept figures from real anchors; not observed market prices.
3. **Cadet is free** (chosen for funnel volume; an optional "supporter" donation could replace this).
4. **Auth = Supabase Auth** (chosen for RLS integration + solo simplicity over Better Auth's richer RBAC).
5. **RON-primary** pricing with EUR secondary.
6. **Fleet & flight booking** are modeled and shown in the CRM as a concept capability, but flight *access/hire* is **not** a launch promise (dues ≠ flying spend).
7. **Brand navy** documented as `#102844`; confirm exact value from the source vector.
8. Concept-grade compliance; **no real legal/tax/payment contracts** in scope.

---

## 14. Source grounding

This foundation is distilled from a 7-stream research pass (Romanian/EASA landscape, membership models, member portals & digital cards, payments/GDPR, solo-dev stack, CRM/fleet modeling, brand/bilingual UX). Full findings: `tasks/wwbq1l2ti.output` (research transcript). Specific facts (license hours, validity windows, prices, vendor tiers) are point-in-time and should be re-verified before any public/legal use.
