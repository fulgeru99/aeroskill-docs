# Aeroskill Club Docs

> Build documentation for a general aviation membership platform.

Aeroskill Club helps people join, participate in, and benefit from a credible aviation club — while giving the club a structured CRM foundation for members, partners, sponsors, flight schools, aerodromes, associations, benefits, contracts, communications, and fleet records.

## Documentation sets

Three independent, complete specification suites for the same platform, written by three different AI authoring runs. They deliberately do not share content.

- 📘 **[Opus](Opus/README.md)** *(formerly "Claude")* — the most exhaustively specified suite: 114 ID'd requirements with Given/When/Then criteria, ~43 tables, 57 diagrams. Self-described concept/portfolio piece with its own pricing model (free / 490 / 1.490 RON).
- 📙 **[Fable](Fable/README.md)** — the research-grounded suite: follows the locked brief (3 tiers at 3000 / 4500 / 6000 RON/year), 53 cited sources (Romanian law, market prices, payment fees, accessibility standards), current stack (Next.js 16), delivered-logo brand integration.
- 📗 **[Codex](Codex/README.md)** — the broadest suite: ~250–300 requirements, 64 ASCII wireframes, strong schema and flow craft; leaves prices, currency, and jurisdiction as open questions.

## How the three suites compare

A cross-review (July 2026) profiled all three suites against the same 10-dimension rubric, with spot-verification of factual claims. Summary:

### The suites answer different briefs

| | Tiers & prices | Follows the "3000/4500/6000 RON, 3 tiers" brief? |
|---|---|---|
| **Opus** | Cadet **free** · Aviator **490 RON/yr** · Captain **1.490 RON/yr** (+ 4.990 RON lifetime) — flagged as "concept figures… assumed, not user-specified" | No — replaced the model (free funnel tier + anchor pricing) |
| **Fable** | Cadet **3000** · Pilot **4500** · Captain **6000 RON/yr**, locked, with break-even math at researched Romanian prices | **Yes** |
| **Codex** | Three tiers; names "recommended", prices explicitly deferred in every document | No — deferred |

### Objective metrics

| Metric | Opus (6,489 lines) | Fable (2,614 lines) | Codex (11,188 lines) |
|---|---|---|---|
| Mermaid diagrams | **57** | 13 | 9 |
| External citation links | ~0 | **53** (26 domains: laws, market, standards) | 11 (vendor docs) |
| Requirements with unique IDs | **114** (Given/When/Then) | 86 (bullet criteria) | ~250–300, no IDs |
| Database tables | **~43** (+44 enums) | 20 (+ SQL policy sketches) | 29 |
| Substance density (table-rows / 1k lines) | 256 | **263** | 72 |
| Traceability density (ID-mentions / 1k lines) | 100 | **236** | 0 |

### Scorecard (1–5 per dimension)

| Dimension | Opus | Fable | Codex |
|---|---|---|---|
| Decision-lockedness | 5 | 5 | 3 |
| Research grounding | 3.5 (real, uncited) | 4.5 (cited) | 2 |
| PRD rigor | **5** | 4.5 | 3 |
| Data model | **5** | 4.5 | 5 |
| Flows | **5** | 4 | 5 |
| Design system | **5** | 4.5 | 4 |
| Infra / GDPR | 4.5 | **5** | 2 |
| Planning | 5 | 4.5 | 4 |
| Internal consistency | 4.5 | **5** | 4 |
| Signal density | 5 | **5** | 4 |

### Verdict

- **Opus** wins on *specification craft*: the deepest per-artifact detail (GiST booking constraints, measured WCAG contrast matrix, ~60 flow edge cases) and genuinely deep aviation-regulatory research (SAUM vs AACR, ARC renewal rules) — but with zero citations, its own invented pricing, GDPR retention left unresolved, and a route-naming drift between its docs 05 and 09.
- **Codex** wins on *volume and breadth*: wireframes and a test-scenario catalog no other suite has — but the business-critical decisions (price, currency, jurisdiction) are never made, there is no regulatory grounding, and ~20–25% of the text is duplication (docs 01–03 are raw chat transcripts).
- **Fable** wins on *verification and brief-fidelity*: the only suite whose external claims are researched and cited (OG 26/2000 statutory tier categories, e-Factura deadlines, Law 36/2023 retention, EAA analysis, real Stripe/Netopia fees, real PPL/rental prices), the only one on the current stack, the only one with the delivered logo integrated — at roughly 2–3× less artifact detail than Opus.

One-line summary: **Opus specified the most, Codex wrote the most, Fable verified the most.**

## Adding pages

1. Drop a `.md` file into `docs/Opus/`, `docs/Fable/`, or `docs/Codex/`.
2. Link it in [`docs/_sidebar.md`](_sidebar.md) under the matching section.
3. Commit and push — it's live.
