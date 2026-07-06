# 04 — Product Requirements Document (PRD)

> **Purpose:** the complete v1 requirements for all three surfaces, with stable IDs and acceptance criteria. Inherits `00-foundation.md` (tiers, prices, statuses, roles, conventions — never restated as decisions here) and `01`/`02` (personas, strategy). Downstream: `05` (routes), `06` (schema), `07` (flows), `03`/`10` (sequencing) all cite these IDs.

**How to read:** every requirement has an ID (`PUB-`/`MEM-`/`ADM-`/`PLT-`, per 00 §7.1), a MoSCoW priority (**M**ust / **S**hould / **C**ould), and testable acceptance criteria (AC). IDs are assigned once and never reused. "Member" always means a person with role `member`; statuses always use the locked vocabularies (00 §7.2).

---

## 1. Public website (PUB)

### 1.1 Content pages

**PUB-001 — Home page** (M)
Landing page presenting the club promise, tier teaser, Gold sponsors, and a Join call to action.
- AC1: Renders hero with mission one-liner (01 §1), a 3-tier price teaser (3000/4500/6000 RON, locale-formatted per 00 §7.3), and an accent Join button linking to `/join`.
- AC2: Displays Gold-package sponsor logos when at least one sponsor with an `active` sponsorship contract and `visible_on_site = true` exists; section hidden otherwise.
- AC3: Fully server-rendered; LCP < 2.5 s on mid-range mobile (00 §8).

**PUB-002 — Mission page** (M)
The "why we exist" page.
- AC1: Presents mission, the problem (01 §2), and club legal identity (asociație, 00 §2).
- AC2: Available in `ro` and `en` with full content parity.

**PUB-003 — Membership page with tier comparison** (M)
The conversion page.
- AC1: Shows all three tiers side by side with locked names and prices and the benefit-class comparison of 02 §3.
- AC2: Pilot tier is visually recommended (08 §5 pricing card).
- AC3: Each tier links to `/join?tier={slug}` preselecting that tier.
- AC4: Prices render locale-formatted; no other currency shown.

**PUB-004 — Tier benefits rendered from live data** (S)
The comparison's concrete benefit rows come from the database, not hardcoded copy.
- AC1: Published benefits (`benefits.active = true` with qualifying contract, ADM-022) appear grouped by partner type with their `min_tier` mapping.
- AC2: A benefit unpublished in the CRM disappears from the public page without a deploy.

**PUB-005 — Founding-member counter** (S)
Scarcity indicator for the founding offer (00 §3.5).
- AC1: Shows remaining founding slots (50 − approved founding members), live from the database.
- AC2: Section disappears automatically once slots reach 0.

**PUB-006 — Sponsors page** (M)
- AC1: Sponsors grouped by package (Gold, Silver, Bronze) with logo, name, and outbound link (`rel="sponsored"`).
- AC2: Only sponsors with an `active` sponsorship contract and `visible_on_site = true` appear.
- AC3: Empty state ("Become a sponsor" pitch + contact CTA) when no sponsors are visible.

**PUB-007 — Fleet showcase page** (S)
- AC1: Lists aircraft with `status = active` and public visibility enabled (ADM-031): photo, type, registration, base aerodrome.
- AC2: Aircraft in `maintenance`/`retired` or non-public never appear.

**PUB-008 — Contact page** (M)
- AC1: Shows club contact details (from `club_settings`) and a contact form (name, email, message) validated with Zod.
- AC2: Submission emails the club inbox via Resend and shows a success state; spam-protected (honeypot + rate limit per PLT-011).

**PUB-009 — Join entry point** (M)
- AC1: `/join` presents the three tiers and starts the registration/application flow (MEM-001/002) with the chosen tier carried through.
- AC2: A logged-in `active` member visiting `/join` is redirected to their portal dashboard.

**PUB-010 — Legal pages** (M)
- AC1: Privacy policy, terms of membership, and cookie notice exist in both locales at stable URLs (05 §2).
- AC2: Privacy policy lists processors consistent with 09 §GDPR.

### 1.2 Public platform behavior

**PUB-011 — Locale switching** (M)
- AC1: Header locale switcher toggles `ro` (root) ↔ `en` (`/en` prefix) preserving the current page (00 §4.4).
- AC2: `hreflang` alternates emitted on every public page.

**PUB-012 — SEO basics** (M)
- AC1: Unique title/meta description per public page per locale; Open Graph image; `sitemap.xml` and `robots.txt` generated.
- AC2: Portal (`/portal`), admin (`/admin`), and verification (`/verify/*`) routes are `noindex`.

**PUB-013 — Member-card verification page** (M)
The partner-facing live card check (02 R5, 08 §6.5).
- AC1: `GET /verify/{token}` renders, without authentication: verdict (valid member in `active` or `grace` → success; anything else → invalid), first name + last initial, tier badge, validity date, and check timestamp.
- AC2: Unknown or revoked token renders the invalid verdict with no member data and returns within the same layout (no error page).
- AC3: Response is rendered server-side from live data; no caching beyond 60 s.
- AC4: No personal data beyond AC1's fields is ever present in the response (GDPR minimization).

**PUB-014 — Error pages** (M)
- AC1: Branded 404 and 403 pages in both locales with a path back home; portal 403 explains login/role requirement.

---

## 2. Member portal (MEM)

### 2.1 Account & application

**MEM-001 — Registration** (M)
- AC1: Sign-up with email + password (Supabase Auth); email confirmation required before application can be submitted.
- AC2: Password policy: ≥ 10 characters; breach-list check via Supabase settings.
- AC3: Duplicate email produces a clear, non-enumerating error.

**MEM-002 — Membership application** (M)
- AC1: Applicant selects tier, provides required data (full name, phone, county, date of birth, pilot status: enthusiast/student/licensed), and accepts terms; marketing consent is a separate, unticked checkbox (00 §8.1).
- AC2: Submission creates a `members` row with status `pending` and a `memberships` row (chosen tier, status `pending`) and routes to payment (MEM-005/006).
- AC3: All fields Zod-validated with localized messages.

**MEM-003 — Login / logout** (M)
- AC1: Email + password login; session via Supabase Auth cookies; logout invalidates the session.
- AC2: Post-login redirect by role: `member` → `/portal`, `staff`/`admin` → `/admin`.
- AC3: Failed logins are rate-limited (PLT-011) with a non-enumerating message.

**MEM-004 — Password reset** (M)
- AC1: Self-service email reset flow; link single-use and expiring (≤ 1 h).
- AC2: Reset never confirms whether an email exists.

### 2.2 Payment & activation

**MEM-005 — Card payment via Stripe Checkout** (M)
- AC1: From an unpaid `pending` membership, "Pay by card" creates a Stripe Checkout session for the exact tier price in RON.
- AC2: Payment confirmation happens **only** via the signature-verified webhook (PLT-009), which sets the payment `confirmed` and triggers activation (MEM-007).
- AC3: Cancel/failure returns the member to a retry screen with bank-transfer alternative offered; payment recorded as `failed` where a session concluded unsuccessfully.

**MEM-006 — Bank-transfer payment instructions** (M)
- AC1: "Pay by bank transfer" shows club IBAN and beneficiary (from `club_settings`) and payment reference = member number (00 §4.3), plus the exact amount.
- AC2: Creates a `payments` row (`bank_transfer`, `pending`) visible in the admin queue (ADM-002); member sees "awaiting confirmation" status.

**MEM-007 — Activation** (M)
- AC1: A membership becomes `active` only when its payment is `confirmed` **and** the application is approved (ADM-005); whichever completes last triggers activation.
- AC2: Activation sets `starts_on` = activation date, `ends_on` per 00 §3.2, member status `active`, assigns member number (first activation only, format 00 §6), issues the member card (token + card record), and sends the welcome/activation email (PLT-004).
- AC3: First 50 approved members are flagged founding members automatically (00 §3.5).

### 2.3 Portal core

**MEM-008 — Dashboard** (M)
- AC1: Shows membership status chip, tier, validity dates, latest announcements (MEM-020), and context-appropriate primary action (Pay / Renew / View card).
- AC2: Members in `grace` see a warning banner with days remaining and a Renew button (08 §6.3).

**MEM-009 — Profile view & edit** (M)
- AC1: Member can view and edit their own contact data (phone, county, address); name changes flag the member for staff review rather than applying silently.
- AC2: Email change goes through Supabase re-confirmation.
- AC3: Edits are only possible on the member's own row (RLS, 06).

**MEM-010 — Avatar upload** (C)
- AC1: JPEG/PNG ≤ 2 MB to Supabase Storage; shown in portal header and admin member detail.

**MEM-011 — Membership view** (M)
- AC1: Shows current membership (tier, status, `starts_on`/`ends_on`, price paid) and full membership history.

**MEM-012 — Renewal** (M)
- AC1: From T−30 before `ends_on` through end of grace, a Renew action offers same-tier renewal (or downgrade per 00 §3.3) with both payment methods.
- AC2: Renewal creates the next `memberships` row starting at previous `ends_on + 1 day` (no gap, 00 §3.2), activated on payment confirmation.
- AC3: After grace ends unpaid, status transitions to `expired` (PLT-006) and the portal shows a rejoin path.

**MEM-013 — Tier upgrade** (M)
- AC1: An `active` member can upgrade mid-year; the price shown is the pro-rated difference per 00 §3.3, computed server-side.
- AC2: On payment confirmation the current membership's tier updates immediately; card and benefits reflect the new tier at once.
- AC3: Downgrade is not offered mid-year (renewal-time only).

**MEM-014 — Payment history** (M)
- AC1: Lists own payments (date, amount, method, status) with downloadable payment-confirmation PDF for `confirmed` ones.
- AC2: Confirmation PDF explicitly states it is not a fiscal invoice (00 §2).

### 2.4 Member card

**MEM-015 — Digital member card** (M)
- AC1: `/portal/card` renders the card exactly per 08 §6 (name, member number, tier, validity, QR of `/verify/{token}`).
- AC2: Card state follows member status (active / grace / expired renderings, 08 §6.3).
- AC3: QR scans successfully from another phone's camera at typical desk distance.

**MEM-016 — Card offline tolerance** (S)
- AC1: Card page loads last-known card from cache without network; verification itself always requires live lookup (PUB-013).
- AC2: Portal offers "add to home screen" guidance.

### 2.5 Benefits & communication

**MEM-017 — Benefits catalog** (M)
- AC1: Lists published benefits with partner name, description, and how to redeem (show card); benefits above the member's tier appear locked with an upgrade prompt.
- AC2: Catalog matches exactly what ADM-021/022 publish.

**MEM-018 — Benefit filtering** (S)
- AC1: Filter by partner type (`flight_school` / `association` / `aerodrome` / `sponsor`) and by "available to my tier".

**MEM-019 — Communication preferences** (M)
- AC1: Marketing consent toggle (default off, timestamped); transactional email is labeled non-optional.
- AC2: Campaign sends (ADM-026) exclude members without consent; every marketing email contains an unsubscribe link that flips this toggle.

**MEM-020 — Announcements feed** (S)
- AC1: Portal lists published announcements (campaigns of kind `announcement`) newest-first; dashboard shows the latest.

### 2.6 GDPR self-service

**MEM-021 — Data export** (M)
- AC1: One-click export produces a machine-readable JSON of the member's personal data, memberships, payments, and consents, delivered as a download.
- AC2: Export is audit-logged (PLT-007).

**MEM-022 — Erasure request** (M)
- AC1: Member can request account deletion; request is queued for admin execution (ADM-035) and confirmed by email.
- AC2: Portal states the legal retention carve-outs (payment records) per 09 §GDPR.

---

## 3. Admin CRM (ADM)

Admin CRM is Romanian-only (00 §4.4). Every mutating action here is audit-logged (PLT-007). Access: `staff` and `admin` roles; requirements marked **[admin]** are `admin`-only (00 §5).

### 3.1 Dashboard & queues

**ADM-001 — Dashboard metrics** (M)
- AC1: Shows active members (total + per tier), members in `grace`, revenue year-to-date (RON, confirmed payments), renewal rate rolling 12 months, active contracts, sponsors live on site.
- AC2: Numbers reconcile with their underlying list views.

**ADM-002 — Action queues** (M)
- AC1: Dashboard surfaces: pending applications (ADM-005), pending bank transfers (ADM-006), contracts expiring ≤ 60 days (ADM-020), aircraft documents expiring ≤ 60 days (ADM-030).
- AC2: Each queue item deep-links to the record; empty queues show a zero state.

### 3.2 Member management

**ADM-003 — Member list** (M)
- AC1: Paginated, searchable (name, email, member number) list with filters: status, tier, founding flag, county, marketing consent.
- AC2: Row shows status chip per 08 §5 mapping.

**ADM-004 — Member detail (360°)** (M)
- AC1: One page shows profile data, membership history, payments, card (token status, verification link), consents, and campaign sends received.
- AC2: All member-related actions (ADM-005..010) are launchable from this page.

**ADM-005 — Application review** (M)
- AC1: Staff can approve or reject a `pending` application; approval contributes to activation per MEM-007 AC1.
- AC2: Rejection records a reason and emails the applicant; a rejected applicant's data is purged after 90 days (09 §retention).

**ADM-006 — Manual payment confirmation** (M)
- AC1: Staff can confirm a `pending` bank-transfer payment (recording paid date and bank reference), which triggers activation/renewal exactly as a Stripe confirmation would.
- AC2: Staff can also register an unannounced received transfer against a member and membership.
- AC3: Confirmation is irreversible in UI except by `admin` refund action (payment → `refunded`), both audit-logged.

**ADM-007 — Edit member data** (M)
- AC1: Staff can correct profile fields; changes are audit-logged with before/after values.

**ADM-008 — Archive member** (M)
- AC1: Archiving sets status `archived`, revokes card validity, excludes from campaigns; requires typed confirmation (08 §5 modal rules).
- AC2: Archived members remain visible via an "include archived" filter.

**ADM-009 — Manual membership adjustment** (S)
- AC1: `admin` can extend `ends_on` or correct tier with a mandatory reason; adjustment appears in membership history.

**ADM-010 — Card token reissue** (S)
- AC1: Staff can revoke and reissue a member's verification token (lost/leaked card); old token immediately verifies as invalid.

**ADM-011 — Member export** (S)
- AC1: Filtered member list exports to CSV (UTF-8, semicolon-delimited for RO Excel); export is audit-logged.

### 3.3 Partner management

**ADM-012 — Flight schools CRUD** (M)
- AC1: Create/edit/list flight schools: name, contact person, email, phone, aerodrome(s) of operation, notes, active flag.

**ADM-013 — Associations CRUD** (M)
- AC1: Same pattern as ADM-012 for partner associations (name, scope, contacts, active flag).

**ADM-014 — Aerodromes CRUD** (M)
- AC1: Create/edit/list aerodromes: name, ICAO code (validated 4 letters, unique), county, coordinates (optional), contacts, active flag.

**ADM-015 — Sponsors CRUD** (M)
- AC1: Create/edit/list sponsors: name, package (`bronze`/`silver`/`gold`), logo upload, website URL, contact, `visible_on_site` toggle, display order.
- AC2: Public visibility additionally requires an `active` sponsorship contract (PUB-006 AC2); the form warns when the toggle is on without one.

**ADM-016 — Partner detail** (M)
- AC1: Each partner detail page lists its linked contracts and benefits with statuses and deep links.

### 3.4 Contracts

**ADM-017 — Contracts CRUD** (M)
- AC1: Create/edit contracts with: auto-assigned number (`CTR-YYYY-NNN`, 00 §6), exactly one counterparty (any of the four partner types — enforced), type (`partnership`/`sponsorship`/`service`), `starts_on`/`ends_on`, value (RON, optional), terms summary, status.
- AC2: Contract list filterable by status, type, partner type, expiry window.

**ADM-018 — Contract lifecycle** (M)
- AC1: Transitions follow 00 §7.2: `draft` → `active` (activation requires dates + counterparty); `active` → `terminated` (manual, reason required); `active` → `expired` (automatic at `ends_on`, PLT-006).
- AC2: Status changes cascade per ADM-022 (benefits) and PUB-006 (sponsor visibility).

**ADM-019 — Contract documents** (M)
- AC1: PDF upload (≤ 10 MB) to private storage; download only for `staff`/`admin`.

**ADM-020 — Contract expiry alerts** (M)
- AC1: At 60 and 30 days before `ends_on`, an email alert goes to club staff and the contract appears in the ADM-002 queue.

### 3.5 Benefits

**ADM-021 — Benefits CRUD** (M)
- AC1: Create/edit benefits with: granting partner (one of the four types), optional linked contract, bilingual title/description (`*_ro`/`*_en`), `min_tier`, redemption note, active flag.
- AC2: Benefits list filterable by partner, tier, active state.

**ADM-022 — Benefit publication rule** (S)
- AC1: A benefit renders publicly (PUB-004) and in the member catalog (MEM-017) only while `active = true` **and** its linked contract (when set) is `active`; contract expiry hides it automatically.

### 3.6 Communication

**ADM-023 — Email template management** (S)
- AC1: Seeded lifecycle templates (PLT-004 list) are editable (subject + body per locale) with variable placeholders documented; a test render preview is available.

**ADM-024 — Campaign composer** (M)
- AC1: Compose campaign: kind (`email`/`announcement`), subject/title, rich-text body, audience segment — all members / by tier(s) / by status(es), with live recipient count.
- AC2: Email campaigns automatically exclude members without marketing consent (MEM-019); the count shows both totals.

**ADM-025 — Campaign test send** (S)
- AC1: Send a test to a staff address before scheduling.

**ADM-026 — Campaign send & log** (M)
- AC1: Campaigns can be sent immediately or scheduled; sending records one `campaign_sends` row per recipient with outcome.
- AC2: Status follows 00 §7.2 (`draft` → `scheduled` → `sent`; `cancelled` allowed before sending); a `sent` campaign is immutable.
- AC3: Send failures are visible per recipient with retry for the failed subset.

**ADM-027 — Announcements** (S)
- AC1: Announcement campaigns publish to the portal feed (MEM-020) without email; they can be unpublished.

**ADM-028 — Automated email visibility** (S)
- AC1: Lifecycle emails (PLT-004) appear in a filterable send log (recipient, template, timestamp, outcome).

### 3.7 Fleet

**ADM-029 — Aircraft CRUD** (M)
- AC1: Create/edit aircraft: registration (unique, e.g. `YR-ABC`), manufacturer, model, year, seats, ownership (`owned`/`leased`/`partner`), base aerodrome (FK), status, hourly-rate note, photo.
- AC2: Fleet list shows status chips and base aerodrome.

**ADM-030 — Aircraft document tracking** (S)
- AC1: Per aircraft: ARC expiry and insurance expiry dates; items expiring ≤ 60 days appear in ADM-002 queue and email alert.

**ADM-031 — Fleet public visibility** (S)
- AC1: Per-aircraft public toggle controls PUB-007 listing.

### 3.8 System [admin]

**ADM-032 — Staff & admin user management** (M) [admin]
- AC1: `admin` can invite staff (email invite → account with role `staff`), change roles, and deactivate accounts; cannot demote the last `admin`.

**ADM-033 — Club settings** (M) [admin]
- AC1: Editable singleton: club contact details, IBAN + beneficiary for transfers, addresses used in emails and legal pages.

**ADM-034 — Audit log view** (M) [admin]
- AC1: Filterable log (actor, entity, action, date range) of all mutations captured by PLT-007; read-only.

**ADM-035 — GDPR erasure execution** (M) [admin]
- AC1: `admin` executes queued erasure requests (MEM-022): personal data anonymized in place, auth account deleted, payment records retained in anonymized form (legal basis, 09 §GDPR).
- AC2: Execution is irreversible, typed-confirmation gated, and audit-logged; requester receives a completion email before their address is erased.

---

## 4. Platform / cross-cutting (PLT)

**PLT-001 — Authentication** (M)
- AC1: Supabase Auth (email + password, confirmation, reset) is the only authentication path; sessions are HTTP-only secure cookies.

**PLT-002 — Authorization (RBAC + RLS)** (M)
- AC1: Route-level guards per the 05 §5 access map (visitor/member/staff/admin) in Next.js middleware and server actions.
- AC2: Every table has RLS enabled with deny-by-default policies per 06 §RLS; direct API access cannot exceed role rights even if UI guards fail.

**PLT-003 — i18n mechanics** (M)
- AC1: Locale routing per 00 §4.4; all UI strings from next-intl catalogs (no hardcoded copy); bilingual content fields render by locale with `ro` fallback.
- AC2: Admin CRM renders `ro` only.

**PLT-004 — Transactional email set** (M)
- AC1: The following templated emails send automatically via Resend: email confirmation, application received, application rejected, bank-transfer instructions, payment confirmed, membership activated (with card link), renewal reminders at T−30/T−7/T0/T+14/T+30 (00 §3.2), upgrade confirmed, erasure request received, erasure completed, staff invite; plus staff alerts: contract expiry (ADM-020), aircraft documents (ADM-030), new pending transfer (ADM-002).
- AC2: Member-facing emails send in the member's locale; staff alerts in `ro`.

**PLT-005 — Scheduled jobs** (M)
- AC1: A daily scheduled job (09 §deployment) runs status transitions (PLT-006), renewal reminders, and expiry alerts; runs are idempotent and logged.

**PLT-006 — Status transition engine** (M)
- AC1: Memberships: `active` → `grace` at `ends_on + 1 day`; `grace` → `expired` at `ends_on + 31 days`; member status mirrors it (00 §7.2).
- AC2: Contracts: `active` → `expired` the day after `ends_on`.
- AC3: All automatic transitions are audit-logged with job identity as actor.

**PLT-007 — Audit logging** (M)
- AC1: Every mutation in the admin CRM and every automatic transition writes an `audit_logs` row (actor, action, entity type + id, before/after diff, timestamp).
- AC2: Audit rows are insert-only (no update/delete via any role).

**PLT-008 — Validation** (M)
- AC1: Every form, server action, and webhook payload is Zod-validated; invalid input never reaches the database.

**PLT-009 — Stripe webhook handling** (M)
- AC1: Webhook signature verified; events processed idempotently (an event id is processed at most once); unknown events acknowledged and ignored.
- AC2: `checkout.session.completed` marks the payment `confirmed` and triggers MEM-007/012/013 effects exactly once.

**PLT-010 — Analytics** (S)
- AC1: Plausible on public + portal pages; no analytics on `/verify/{token}` beyond aggregate hit count; no personal data in events.

**PLT-011 — Rate limiting** (M)
- AC1: Login, password reset, contact form, and `/verify/*` are rate-limited per IP; limits logged when tripped.

**PLT-012 — Empty & error states** (S)
- AC1: Every list view has a designed empty state (08 §5); every server action failure surfaces a localized, non-technical error toast.

---

## 5. Non-functional requirements

| Area | Requirement |
|------|-------------|
| Performance | Public pages LCP < 2.5 s mid-range mobile; portal interactive < 3 s (00 §8); admin lists paginate at 50 rows |
| Availability | Single-region managed stack (09); target 99.5% monthly; verification page is the availability-critical path |
| Backup | Daily automated Postgres backups, 30-day retention, restore drill before public launch (09) |
| Security | 00 §8 baseline: HTTPS-only, RLS-everywhere, signed webhooks, secrets in env, rate limits (PLT-011) |
| Accessibility | WCAG 2.1 AA on public + portal (08 §7) |
| Data volumes | Designed for ≤ 5,000 members, ≤ 200 partners/contracts, ≤ 50 aircraft — no premature scaling work |

## 6. Out of scope

The v1 exclusion list is locked in 00 §9 and repeated here by reference — notably: no native apps, no flight booking/scheduling, no events ticketing, no benefit-redemption tracking, no auto-recurring payments, no fiscal e-invoicing integration, no sponsor self-service. Anything not covered by an ID in this document is out of scope for v1.
