# Fable — Aeroskill Club Platform Specification

Fable is the third, self-contained specification suite for the **Aeroskill Club** web platform — a Romanian general-aviation club with a public website, a member portal, and an admin CRM — written to be built end-to-end by a **single developer working with Claude Code**. It shares no content with the earlier `Claude/` and `Codex/` suites; every decision was made fresh and locked in `00-foundation.md`.

## Reading order

| # | Document | One-liner |
|---|----------|-----------|
| 00 | [Foundation](00-foundation.md) | The canonical decisions everything else inherits — read first |
| 01 | [Product Vision](01-product-vision.md) | Why we exist, who we serve, what success looks like |
| 02 | [Product Strategy](02-product-strategy.md) | Positioning, tier & sponsor strategy, monetization, GTM, risks |
| 03 | [Implementation Plan](03-implementation-plan.md) | Build philosophy, scope slices, milestones, solo Claude Code workflow |
| 04 | [PRD](04-prd.md) | Detailed requirements per surface, with IDs & acceptance criteria |
| 05 | [Information Architecture](05-information-architecture.md) | Sitemap, navigation, URL/i18n structure, RBAC access map |
| 06 | [Database Schema](06-database-schema.md) | Entities, columns, relationships, ERD, RLS posture |
| 07 | [User Flows](07-user-flows.md) | Step-by-step flows across all three surfaces |
| 08 | [Design System](08-design-system.md) | Logo-driven brand, tokens, type, components, member card |
| 09 | [Technical Infrastructure](09-technical-infrastructure.md) | Stack, services, environments, security/GDPR, deployment |
| 10 | [Roadmap](10-roadmap.md) | Sequenced solo Claude Code build roadmap, phase by phase |

**Writing order differed from reading order:** the suite was authored in dependency order — `00 → 01 → 02 → 08 → 04 → 05/06 → 07 → 09 → 03 → 10` — because 03 and 10 plan over the full scope and had to be written last, even though they read early.

## Headline locked facts (from 00)

- **Club:** Aeroskill Club, Romanian non-profit association (*asociație*), general-aviation focus, Romanian-first with English (`ro` default, `/en` prefix).
- **Tiers:** **Cadet — 3000 RON/year · Pilot — 4500 RON/year · Captain (Căpitan) — 6000 RON/year**, anniversary-based membership with a 30-day grace period.
- **Surfaces:** public website · member portal (profile, subscription, payment, digital member card with QR verification) · admin CRM (members, flight schools, associations, aerodromes, sponsors, benefits, contracts, communication, fleet).
- **Stack:** Next.js 15 + Supabase (Postgres/RLS, Auth, Storage) + Tailwind/shadcn + next-intl + Stripe (with bank-transfer fallback) + Resend + Vercel + Plausible.
- **Roles:** visitor · `member` · `staff` · `admin`.

## Using this suite with Claude Code

Start every build session from `03-implementation-plan.md` — it defines the scope slices, which Fable docs to load as context per slice, the branch/commit conventions, and the spec-update rule that keeps these documents and the code from drifting. `10-roadmap.md` tells you which slice is next.
