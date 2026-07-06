# 03 — Implementation Plan

> **Purpose:** how one developer, working with Claude Code, builds the platform — philosophy, scope slices, milestones, and working method. Reads third; **written after** `04`–`09` so its slices reference the real requirements (04), routes (05), tables (06), and pipeline (09). `10-roadmap.md` sequences these slices into phases.

---

## 1. Build philosophy

1. **Spec-first, specs stay true.** The Fable documents are the source of truth. When implementation reveals a better decision, the spec is updated **in the same PR** as the code ("spec-update rule"). Divergence between doc and code is a bug.
2. **Vertical slices over horizontal layers** (01, principle 7). Every slice ends with something demoable end-to-end on production infrastructure — never "the API is done but has no UI".
3. **Every schema change is a migration.** No dashboard-edited schema, ever; CI fails on drift (09 §8). Migration order per 06 §6.
4. **Deploy from day one.** S0 puts the walking skeleton on Vercel + Supabase production before any feature exists; there is no "integration phase".
5. **Boring, few, managed** (01, principle 5). Any proposed new dependency or service must displace one or answer a locked requirement.
6. **Testing posture — automate the money and the math, eyeball the pixels:**
   - **Vitest (unit):** pro-rata upgrade math (00 §3.3), membership date arithmetic and status transitions (PLT-006), segment resolution (ADM-024), Zod schemas. These are the bug-costly parts.
   - **Playwright (smoke):** the golden path FLOW-01 in Stripe test mode, login, card verification page — run in CI against preview (09 §8).
   - **Manual per-slice verification:** each slice's PRD acceptance criteria walked through on the preview URL before merge; visual/UX review is human.
   - No coverage targets; tests exist where regressions are expensive (payments, statuses, RLS).
7. **RLS proves itself.** Each slice that adds tables adds a small RLS assertion script (deny-by-default checks run as anon/member/staff) — cheap insurance that PLT-002 AC2 stays true.

## 2. Scope slices

Effort unit: one Claude Code working session ≈ a focused half-day.

| Slice | Goal | PRD IDs | Tables (06) | Routes (05) | Sessions |
|-------|------|---------|-------------|-------------|----------|
| **S0 — Walking skeleton** | Repo, Next.js 15 + TS strict, Tailwind/shadcn with 08 tokens, next-intl scaffold (ro/en), Supabase project + local stack, CI (lint/type/test/migration-drift), Vercel deploy, error pages | PLT-003 (partial), PUB-014 | migrations 001–002 (`profiles`, `club_settings`) | layout shells, `/`(placeholder) | 3–4 |
| **S1 — Public website** | All static-content public pages, bilingual, SEO, contact form | PUB-001 (static teaser), PUB-002, PUB-003 (seeded tiers), PUB-008, PUB-009 (entry only), PUB-010, PUB-011, PUB-012, PUB-014 | `tiers` seed (mig 003 partial) | `/`, `/mission`, `/membership`, `/contact`, `/join`, `/legal/*` | 4–6 |
| **S2 — Auth & application** | Register/login/reset; application form; portal shell with status states | MEM-001, MEM-002, MEM-003, MEM-004, PLT-001, PLT-002, PLT-008 | `members`, `memberships` (mig 003) + first RLS | `/register`, `/login`, `/reset-password`, `/portal`, `/portal/apply`, `/portal/profile` | 4–6 |
| **S3 — Payments & activation** | Stripe Checkout + webhook; bank-transfer path; activation engine; lifecycle emails (application/payment/activation set); *minimal* approve action for alpha | MEM-005, MEM-006, MEM-007, PLT-009, PLT-004 (subset), ADM-005 (minimal) | `payments`, `stripe_events`, `member_cards`, `email_log` (mig 004–005) | `/portal/membership/pay`, `/api/webhooks/stripe` | 5–7 |
| **S4 — Portal & card** | Dashboard, membership view, renewal & upgrade actions, payment history, the card, public verification | MEM-008–MEM-016, PUB-013 | `verify_card()` RPC (mig 005) | `/portal/*` (membership, renew, upgrade, payments, card), `/verify/{token}` | 4–6 |
| **S5 — Admin core** | CRM shell + queues, member management, manual transfer confirmation, staff/users, settings, audit | ADM-001–ADM-011, ADM-032, ADM-033, ADM-034, PLT-007 | `audit_logs` (mig 011) | `/admin`, `/admin/members(/{id})`, `/admin/users`, `/admin/settings`, `/admin/audit` | 6–8 |
| **S6 — Partners, contracts, benefits** | Four partner CRUDs, contracts lifecycle + documents + alerts (queue only until S7 cron emails), benefits + publication rule; public sponsors/benefits go live | ADM-012–ADM-022, PUB-004, PUB-005, PUB-006, MEM-017, MEM-018 | `flight_schools`, `flight_school_aerodromes`, `associations`, `aerodromes`, `sponsors`, `contracts`, `benefits` (mig 006–008) | `/admin/flight-schools|associations|aerodromes|sponsors|contracts|benefits` (+`/{id}`), `/sponsors` live, `/membership` live rows | 6–8 |
| **S7 — Communication & automation** | Templates, campaigns, announcements, send log; cron engine: status transitions, dunning, expiry alerts | ADM-023–ADM-028, MEM-019, MEM-020, PLT-004 (full), PLT-005, PLT-006 | `email_templates`, `campaigns`, `campaign_sends` (mig 010) | `/admin/campaigns(/{id})`, `/admin/templates`, `/admin/send-log`, `/portal/announcements`, `/api/cron/daily` | 5–7 |
| **S8 — Fleet, GDPR, hardening** | Fleet CRUD + public page; GDPR export/erasure; rate limiting, analytics, empty states, accessibility pass, restore drill | ADM-029, ADM-030, ADM-031, PUB-007, MEM-021, MEM-022, ADM-035, PLT-010, PLT-011, PLT-012 | `aircraft` (mig 009) | `/admin/fleet(/{id})`, `/fleet`, `/portal/settings` | 5–7 |

Total: **42–59 sessions**. Slices are strictly ordered — each depends on the previous (S6 needs S5's CRM shell; S7 needs S6's contracts for expiry emails).

## 3. Milestones

| Milestone | Slices | Exit criteria (Definition of Done) |
|-----------|--------|-------------------------------------|
| **M1 — Private alpha** | S0–S4 | 10–15 seed members complete FLOW-01 with real money (card **and** at least one bank transfer, confirmed via minimal approve); every member holds a working card verified at least once via `/verify/{token}` from a real phone; smoke tests green in CI |
| **M2 — Founding launch** | S5–S6 | CRM operational: staff run FLOW-09/10/11/12/16 without developer help; ≥ 2 real partner contracts and ≥ 1 sponsor live on the public site; founding counter (PUB-005) live; audit log capturing all mutations |
| **M3 — Public launch** | S7–S8 | Dunning cron proven on a test cohort (FLOW-04 end-to-end in staging); campaign sent to real members (FLOW-14); GDPR export + erasure executed once for real; 10 §launch-readiness checklist fully green |

## 4. Solo Claude Code workflow

**Session cadence.** One slice-scoped goal per session. A session = pick the next unchecked item from the slice → implement → verify → PR → merge. Never two slices in flight.

**Context feeding per slice.** Every session starts by loading `00-foundation.md` plus the slice's relevant docs — S1: 04 (PUB) + 05 + 08; S2–S4: 04 (MEM/PLT) + 05 + 06 + 07 (flows being built); S5–S8: 04 (ADM) + 06 + 07. A repo `CLAUDE.md` points at `docs/Fable/` and states the spec-update rule so any session can re-anchor.

**Branch & commit conventions.** Trunk-based: short-lived branches `s{n}/{topic}` (e.g. `s3/stripe-webhook`) off `main`, merged via PR same-or-next day. Conventional commits (`feat:`, `fix:`, `chore:`, `spec:`); `spec:` commits carry Fable-doc updates, satisfying the spec-update rule reviewably.

**Review-verify loop (before every merge).**
1. CI green (lint, types, unit, migration drift, smoke on preview — 09 §8).
2. Walk the slice's PRD acceptance criteria on the preview URL; check the flow against 07 step by step.
3. RLS assertion script for any new table (§1.7).
4. Claude Code self-review pass on the diff (correctness + simplification) before human merge.

**Keeping specs and code honest.** The PR template has a checkbox: "Fable docs updated or confirmed unaffected." Renaming a route, table, or status without updating 05/06/00 fails review by convention.

## 5. Cut lines

If a milestone is running late, drop in this order (never cut below the line of its own exit criteria):

- **M1:** MEM-016 (offline card) → MEM-010 (avatar) → MEM-013 (upgrade — defer to M2) → PUB-012 OG polish.
- **M2:** ADM-011 (CSV export) → ADM-009 (manual adjustment) → PUB-005 (founding counter) → ADM-022 automation (staff toggle manually).
- **M3:** ADM-023 template editing UI (edit seeds in repo instead) → MEM-020/ADM-027 (announcements) → ADM-030 (aircraft doc alerts) → PLT-010 (analytics).

Never cut: payments correctness (PLT-009), activation/status engine (PLT-006), card verification (PUB-013), GDPR self-service (MEM-021/022 — legal), audit log (PLT-007), RLS (PLT-002).

## 6. Sequencing risks

| Risk | Mitigation |
|------|------------|
| Stripe onboarding for the *asociație* stalls (02 R2) | Start Stripe activation paperwork during S0; S3 builds bank-transfer path first so alpha can run on transfers alone |
| Logo/final brand arrives late (08 §1) | Placeholder palette is complete; swap is a token-file change |
| Legal-page content (PUB-010) needs a lawyer | Commission during S1; ship with reviewed drafts no later than M2 |
| Approval flow blocks alpha activations (ADM-005 full CRM lands in S5) | S3 ships the minimal approve action explicitly for this |
| Cron/dunning correctness is date-sensitive | Unit-test the transition engine exhaustively (§1.6); staging cohort with compressed clock before M3 |
