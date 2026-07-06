# 02 — Product Strategy

> **Purpose:** how Aeroskill Club wins — positioning, tier & sponsor strategy, monetization, go-to-market, and risks. Inherits `00-foundation.md` (all prices, tier names, policies) and `01-product-vision.md` (personas, targets).

---

## 1. Positioning

**For** Romanian general-aviation pilots, students, and enthusiasts **who** pay rack rate everywhere and belong nowhere, **Aeroskill Club** is a national GA membership club **that** converts annual dues into negotiated partner benefits, a verifiable member card, and a real community — **unlike** school-bound loyalty schemes or informal groups, it is neutral across schools and aerodromes, and every promised benefit is secured by a signed contract.

### Alternatives landscape (researched — real organizations)

| Alternative | Who / what it is | What it offers | Our wedge |
|-------------|------------------|----------------|-----------|
| Doing nothing | The default | Rack rates, no community | Show the math: benefits recoup dues (§3) |
| **AOPA Romania** | Advocacy association, IAOPA member since 2006 ([aopa.ro](https://www.aopa.ro/)) | Representation, safety education ("Educație, nu legislație") | We are complementary, not competitive: they lobby, we negotiate discounts. Dual membership is natural — pursue a partnership contract, not a rivalry |
| **Aeroclubul României** | State aeroclub, ~11 territorial branches; **free** gliding/parachuting/ultralight courses for ages 15–23 | Subsidized entry into airsports; PPL(A) at cost | We start where the subsidy stops: their alumni hit full GA prices at 24 — the exact moment Cadet membership becomes rational (01 §2) |
| Facebook/WhatsApp groups | Informal school- and aerodrome-centric groups | Chatter, occasional tips | Structure: verified membership, contracted benefits, curated comms |
| Single flight-school loyalty | e.g. house discounts at private schools such as the Tuzla (`LRTZ`) operation | Discounts at one school | Neutrality: benefits across many schools + aerodromes; portable card |

Our defensible asset is the **two-sided network**: members make partner contracts worth signing; contracted benefits make membership worth renewing. No named player occupies the neutral buying-club position today.

### Pricing benchmark (researched)

Membership organizations pilots already know charge an order of magnitude less than our tiers — because they sell *representation*, not *purchasing power*:

| Organization | Annual dues | What dues buy |
|--------------|-------------|---------------|
| AOPA US | $59 / $99 / $179 (~300–900 RON) | Advocacy, media, helplines |
| AOPA Romania | Not publicly listed | Advocacy, community |
| **Aeroskill Club** | **3000 / 4500 / 6000 RON** | **Contracted discounts + card + community** |

The conclusion is a strategic constraint, not a vanity claim: at 6–10× advocacy-level dues, **the public site must lead with break-even math** (§3), and every tier's contracted benefit pool must clear its dues at realistic usage — otherwise renewals (§5) collapse in Year 2.

## 2. Tier strategy

Three tiers (Cadet 3000 / Pilot 4500 / Captain 6000 RON — locked, 00 §3.1) form a commitment ladder:

- **Why three:** one price excludes either enthusiasts or patrons; four+ splinters benefit negotiation. Three maps cleanly to the personas: enthusiast/student → Cadet, active pilot → Pilot, power user/patron → Captain. Legally, the three tiers are statutory member categories with general-assembly-set dues (00 §2) — a fourth tier would also mean a statute amendment.
- **Price psychology (grounded in researched prices):** 3000 RON (~€600) ≈ four wet rental hours — small against a €8,000–10,000 (~40,000–50,000 RON) license, and recouped outright by a single contracted training-package discount (§3). 4500 RON is a 1.5× step justified for pilots who actually fly: the enhanced-discount pool targets break-even at roughly half of a typical renter's annual hours. 6000 RON is deliberately only 2× Cadet: Captain is positioned as "everything, first, everywhere," priced as patronage rather than luxury.
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

### Benefit economics (the honest break-even math)

Because dues are 6–10× advocacy-club levels (§1), every tier must clear its price at *realistic* usage. Worked examples at researched Romanian prices (€1 ≈ 5 RON; rental €135–145/h wet; PPL package €8,000–10,000):

| Member profile | Contracted benefits used | Annual value | vs. dues |
|----------------|--------------------------|--------------|----------|
| Student (Cadet, 3000 RON) | 10% off remaining training package (€5,000 left) | ~€500 (2,500 RON) + events/newsletter | ≈ break-even on the discount alone; ahead with any event |
| New student (Cadet) | 10% off a full €9,000 PPL enrollment | ~€900 (4,500 RON) | **1.5× dues** — the killer conversion story for the school desk |
| Enthusiast (Cadet) | Events, fly-in access, shop/partner perks, community | Subjective | Sold on belonging, not arithmetic — never promise them break-even |
| Renter, 40 h/yr (Pilot, 4500 RON) | 10–15% off rentals (€14–21/h) + waived landing fees at partner aerodromes + fleet preferential rates | €560–840 rental + fees | Break-even at **~20–30 h/yr** — half to three-quarters of actual usage |
| Captain (6000 RON) | Everything above + all events + 4 guest passes + first fleet access | Exceeds Pilot value | Priced as patronage; arithmetic is secondary by design |

**Contracting rule derived from this table (binding for ADM negotiations):** a tier's contracted benefit pool must let its *median target member* recoup dues at **≤ 50% of typical usage**. Training-package percentage discounts are the single highest-leverage benefit to negotiate first — schools give margin on a €9,000 package far more readily than on hourly rates, and one such contract makes Cadet self-evidently rational for every student in Romania.

## 4. Sponsor strategy

Packages Bronze 10,000 / Silver 25,000 / Gold 50,000 RON per year (guide prices, locked 00 §3.4).

- **Who sponsors:** avionics and pilot-supply retailers, fuel providers, insurance brokers, maintenance shops, flight schools buying visibility beyond their partnership, non-aviation premium brands reaching an affluent audience.
- **What they buy:** a verified, engaged GA audience. Deliverables scale by package: public site logo (Bronze) → member-communication presence + 1 campaign mention (Silver) → homepage placement, event presence, 2 campaign mentions (Gold).
- **CRM-backed delivery:** every sponsorship is a `contract` (type `sponsorship`); deliverables are tracked as contract terms; campaign mentions are executed through the communication module, so delivery is provable at renewal.
- **Sourcing motion:** administrator-led. Target 1 Gold anchor early (credibility), fill Bronze from member-adjacent businesses. Sponsor logos appear only while the contract is `active` and `visible_on_site = true`.

## 5. Monetization model

Revenue = membership dues + sponsorships. Costs are structurally negligible and now verified: platform ~480 RON/month (09 §3), card fees ≈ 1.5% + 1 RON per Stripe EEA transaction (~46 RON on a Cadet payment; Netopia plan B would be 0.99% + 0.30 RON — 00 §4.3): **two Cadet memberships cover a year of infrastructure.** The real cost is administrator time — which is exactly what the CRM minimizes.

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
| R11 | Fiscal compliance friction: sponsorship invoices are B2B and must flow through ANAF e-Factura (mandatory for NGOs with economic activity since 2025-07-01) | M | M | Boundary locked in 00 §2: accountant issues sponsor invoices via SPV outside the platform; e-Factura workflow confirmed with the accountant before the first sponsor contract (10 §3) |
| R12 | Statute/tier mismatch: dues per tier are only lawful if tiers exist as statutory member categories with general-assembly-set amounts (OG 26/2000) | L | H | Statute drafted/amended with the three categories before launch; price changes follow the AG + 60-day-notice path (§7) |

L/I = likelihood / impact (H·M·L). Reviewed quarterly by the administrator.

---

*Sources for the researched claims in this document: [AOPA Romania](https://www.aopa.ro/), [AOPA US membership](https://www.aopa.org/membership), [Aeroclubul României](https://aeroclubulromaniei.ro/page/cursuri-gratuite), Romanian school price lists ([Aviation Academy](https://aviationacademy.ro/tarife-cursuri-personal-navigant/), [Cruiser Aviation](https://cruiseraviation.com/ro/articole/cat-costa-scoala-de-zbor)), [Stripe Romania pricing](https://stripe.com/en-ro/pricing), [Netopia review](https://noda.live/ro/articles/recenzie-netopia-payments), [e-Factura NGO deadlines](https://www.avocatnet.ro/articol_67338/e-Factura-ONG-urile-cultele-%C8%99i-agricultorii-persoane-fizice-scap%C4%83-temporar-de-obliga%C8%9Bia-folosirii-sistemului.html), [OG 26/2000](https://legislatie.just.ro/Public/DetaliiDocument/20740). Full research basis: 00 §10.*
