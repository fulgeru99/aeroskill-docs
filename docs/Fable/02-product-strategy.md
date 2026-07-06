# 02 — Product Strategy

> **Purpose:** how Aeroskill Club wins — positioning, tier & sponsor strategy, monetization, go-to-market, and risks. Inherits `00-foundation.md` (all prices, tier names, policies) and `01-product-vision.md` (personas, targets).

---

## 1. Positioning

**For** Romanian general-aviation pilots, students, and enthusiasts **who** pay rack rate everywhere and belong nowhere, **Aeroskill Club** is a national GA membership club **that** converts annual dues into negotiated partner benefits, a verifiable member card, and a real community — **unlike** school-bound loyalty schemes or informal groups, it is neutral across schools and aerodromes, and every promised benefit is secured by a signed contract.

### Alternatives landscape

| Alternative | What it offers | Our wedge |
|-------------|----------------|-----------|
| Doing nothing | Rack rates, no community | Show the math: benefits recoup dues (§3) |
| Facebook/WhatsApp groups | Chatter, occasional tips | Structure: verified membership, contracted benefits, curated comms |
| Single flight-school loyalty | Discounts at one school | Neutrality: benefits across many schools + aerodromes; portable card |
| Existing aero-clubs/associations | Local, often single-aerodrome focus, manual ops | National scope, digital card with instant verification, professional CRM ops |

Our defensible asset is the **two-sided network**: members make partner contracts worth signing; contracted benefits make membership worth renewing.

## 2. Tier strategy

Three tiers (Cadet 3000 / Pilot 4500 / Captain 6000 RON — locked, 00 §3.1) form a commitment ladder:

- **Why three:** one price excludes either enthusiasts or patrons; four+ splinters benefit negotiation. Three maps cleanly to the personas: enthusiast/student → Cadet, active pilot → Pilot, power user/patron → Captain.
- **Price psychology:** 3000 RON ≈ two flight hours — a rounding error against a 60,000+ RON license, easily recouped via base discounts. 4500 RON is a 1.5× step justified by fleet preferential rates that an active renter recovers in ~15 hours. 6000 RON is deliberately only 2× Cadet: Captain is positioned as "everything, first, everywhere," priced as patronage rather than luxury.
- **Target mix:** ~67% Cadet / 25% Pilot / 8% Captain (01 §5). Cadet is the volume engine; Pilot is the margin engine; Captain is the prestige engine and funds nothing structural (never make solvency depend on Captains).
- **Upgrade path:** in-portal, immediate, pro-rated (00 §3.3). The renewal email at T−30 recommends an upgrade when the member's tier no longer matches obvious usage (post-v1 automation; manual in v1).
- **Downgrade:** at renewal only. No mid-year refunds — keeps accounting trivial for a solo operator.

## 3. Benefit ladder (what justifies each step)

Benefits are contracted per partner (`benefits.min_tier`, 06) but the *public promise* is:

| Benefit class | Cadet | Pilot | Captain |
|---------------|-------|-------|---------|
| Digital member card + verification | ✔ | ✔ | ✔ |
| Partner discounts (base level) | ✔ | ✔ | ✔ |
| Newsletter + announcements | ✔ | ✔ | ✔ |
| Club events & fly-ins | ✔ | priority registration | free entry, all events |
| Enhanced partner discounts | — | ✔ | ✔ |
| Club fleet preferential rates | — | ✔ | first access |
| Guest passes / year | — | 2 | 4 |
| Concierge support | — | — | ✔ |

Rule (from 01, principle 2): **nothing appears in this table publicly until at least one signed contract delivers it.** The public tier page renders from the same data the CRM manages.

## 4. Sponsor strategy

Packages Bronze 10,000 / Silver 25,000 / Gold 50,000 RON per year (guide prices, locked 00 §3.4).

- **Who sponsors:** avionics and pilot-supply retailers, fuel providers, insurance brokers, maintenance shops, flight schools buying visibility beyond their partnership, non-aviation premium brands reaching an affluent audience.
- **What they buy:** a verified, engaged GA audience. Deliverables scale by package: public site logo (Bronze) → member-communication presence + 1 campaign mention (Silver) → homepage placement, event presence, 2 campaign mentions (Gold).
- **CRM-backed delivery:** every sponsorship is a `contract` (type `sponsorship`); deliverables are tracked as contract terms; campaign mentions are executed through the communication module, so delivery is provable at renewal.
- **Sourcing motion:** administrator-led. Target 1 Gold anchor early (credibility), fill Bronze from member-adjacent businesses. Sponsor logos appear only while the contract is `active` and `visible_on_site = true`.

## 5. Monetization model

Revenue = membership dues + sponsorships. Costs (platform ~500 RON/month, 09 §cost table; Stripe fees ~2% on card volume) are structurally negligible: **two Cadet memberships cover a year of infrastructure.** The real cost is administrator time — which is exactly what the CRM minimizes.

### Scenarios (RON, rounded; mix per 01 §5)

| Scenario | Y1 members / revenue | Y2 members / revenue | Y3 members / revenue |
|----------|---------------------|----------------------|----------------------|
| Conservative | 70 / 254,000 | 140 / 508,000 | 250 / 906,000 |
| **Base (targets)** | **120 / 435,000** | **250 / 945,000** | **450 / 1,725,000** |
| Stretch | 180 / 653,000 | 380 / 1,425,000 | 650 / 2,438,000 |

| Sponsor revenue | Y1 | Y2 | Y3 |
|-----------------|----|----|----|
| Conservative | 25,000 (2) | 70,000 (4) | 150,000 (7) |
| **Base** | **60,000 (4)** | **150,000 (8)** | **300,000 (12)** |
| Stretch | 110,000 (6) | 250,000 (11) | 450,000 (16) |

Break-even framing: the platform pays for itself at ~2 members; the *club* (events, negotiation time, admin stipend) is sustainable from ~60 members — below even the conservative Y1 case.

### Renewal economics

Renewal rate is the lever that dominates Y2+ (targets: ≥70% Y2, ≥80% Y3). The platform's renewal machinery — reminder sequence T−30/T−7/T0/T+14/T+30, 30-day grace, no-gap renewal during grace (00 §3.2) — exists because each retained member is worth 3625 RON/year on average versus a ~0 marginal cost to retain.

## 6. Go-to-market

**Sequencing** (mirrors milestones in 03/10):

1. **Private alpha** — 10–15 seed members (friends of the club) join and pay real dues through the platform; 2 partner contracts signed so the benefits page is honest at launch.
2. **Founding-member launch** — the locked offer (00 §3.5): first 50 members get the founding badge + 2-year price lock. Scarcity is real (counter on the public site) and price-integrity-safe (no discount, only a lock).
3. **Public launch** — full site live, ≥5 flight schools, ≥4 aerodromes, ≥2 sponsors contracted; PR push through partner channels.

**Channels, in priority order:**

1. **Partner flight schools as distribution** — the school desk recommends Cadet to every student (the school gains loyal, subsidized customers; we gain P1/Andrei at the moment of maximum motivation).
2. **Aerodrome presence** — fly-in visits, posters with QR to `/join` at partner aerodromes.
3. **Organic social** — Romanian GA Facebook groups, Instagram (aviation photography angle courts P3/Mihai).
4. **The card itself** — every desk verification (`/verify/{token}`) is a live demo in front of non-members.

## 7. Pricing & discount policy

1. Tier prices are public, fixed, and identical for everyone: **3000 / 4500 / 6000 RON**. No percentage discounts, no promo codes in v1.
2. The only price mechanism is the **founding-member 2-year price lock** (00 §3.5).
3. Sponsor guide prices may be negotiated **±20%** per contract; anything beyond requires `admin` sign-off and lives in the contract record.
4. Upgrades pro-rated per 00 §3.3; downgrades at renewal only; dues non-refundable.
5. Price changes apply to *new* membership years only and require 60 days' public notice.

## 8. Risk register

| # | Risk | L | I | Mitigation |
|---|------|---|---|------------|
| R1 | Too few partner contracts at launch → benefits promise rings hollow | M | H | Gate public launch on ≥5 schools + ≥4 aerodromes signed (10 §launch checklist); alpha proves the pitch |
| R2 | Stripe onboarding friction for a Romanian *asociație* | M | H | Bank-transfer fallback is first-class from day one (00 §4.3); Netopia as documented plan B |
| R3 | Renewal underperformance in Y2 | M | H | Renewal machinery in v1 core (not bolted on); track cohort renewal monthly in CRM dashboard |
| R4 | Solo-developer bus factor | M | M | Fable docs as source of truth; boring managed stack; everything reproducible from repo + migrations (03 §philosophy) |
| R5 | Card fraud (screenshots, expired cards) | M | M | Verification is server-side live status at `/verify/{token}` — a screenshot can't fake the verification page (04 MEM/PUB reqs) |
| R6 | GDPR complaint or breach | L | H | Non-negotiables (00 §8): RLS everywhere, minimal data, export/erasure self-service, processor list in 09 |
| R7 | Partner honors benefits inconsistently | M | M | Contracted terms + partner briefing sheet; verification page shows exactly what the tier entitles |
| R8 | Administrator overload as membership scales | M | M | CRM alerts (contract expiry, pending bank transfers) and campaign tooling from v1; 5 h/week budget tracked (01 §5) |
| R9 | A school launches a copycat club | L | M | Neutrality moat — multi-school benefits can't be copied by any single school; move fast on aerodrome coverage |
| R10 | Scope creep (events, booking, e-learning) delays launch | H | M | Locked out-of-scope list (00 §9); cut lines per milestone (03 §cut lines) |

L/I = likelihood / impact (H·M·L). Reviewed quarterly by the administrator.
