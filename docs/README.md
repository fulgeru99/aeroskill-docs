# Aeroskill Club Docs

> Build documentation for a general aviation membership platform.

Aeroskill Club helps people join, participate in, and benefit from a credible aviation club — while giving the club a structured CRM foundation for members, partners, sponsors, flight schools, aerodromes, associations, benefits, contracts, communications, and fleet records.

## Documentation sets

Three independent specification suites were written for the same platform by three different AI authoring runs, then synthesized into a fourth:

- 🏆 **[Combined](Combined/README.md)** — the synthesis suite: takes the deepest treatment of every topic from the three runs under explicit merge rules (brief is law → verified research → deepest craft → breadth). Brief-fit pricing (3000/4500/6000 RON), Fable's research base, Opus's Given/When/Then rigor and contrast matrix, Codex's wireframes and test catalog. **Start here.**
- 📘 **[Opus](Opus/README.md)** *(formerly "Claude")* — the most exhaustively specified original suite: 114 ID'd requirements with Given/When/Then criteria, ~43 tables, 57 diagrams. Self-described concept/portfolio piece with its own pricing model (free / 490 / 1.490 RON).
- 📙 **[Fable](Fable/README.md)** — the research-grounded suite: follows the locked brief (3 tiers at 3000 / 4500 / 6000 RON/year), 53 cited sources (Romanian law, market prices, payment fees, accessibility standards), current stack (Next.js 16), delivered-logo brand integration.
- 📗 **[Codex](Codex/README.md)** — the broadest suite: ~250–300 requirements, 64 ASCII wireframes, strong schema and flow craft; leaves prices, currency, and jurisdiction as open questions.

## How the four suites compare

A cross-review (July 2026) profiled the three original suites against the same 10-dimension rubric, with spot-verification of factual claims; **Combined** was then synthesized from them under explicit merge rules and is measured here with the same yardsticks.

### Objective metrics

| Metric | **Combined** (5,177 lines) | Opus (6,489 lines) | Fable (2,614 lines) | Codex (11,188 lines) |
|---|---|---|---|---|
| Mermaid diagrams | 21 | **57** | 13 | 9 |
| External citation links | **62** (29 domains) | ~0 | 53 (26 domains) | 11 (vendor docs) |
| Requirements with unique IDs | **111** (Given/When/Then) + 18 flows + 32 test scenarios | 114 (Given/When/Then) | 84 (bullet criteria) | ~250–300, no IDs |
| Database tables | 24 (+ 47-index catalog, CRUD matrix, policy SQL) | **~43** (+44 enums) | 20 (+ SQL policy sketches) | 29 |
| Wireframes | 12 (key screens) | 0 | 0 | **64** |
| Substance density (table-rows / 1k lines) | **333** | 256 | 263 | 72 |
| Traceability density (ID-mentions / 1k lines) | **406** | 100 | 236 | 0 |

### Scorecard (1–5 per dimension)

| Dimension | **Combined*** | Opus | Fable | Codex |
|---|---|---|---|---|
| Decision-lockedness | 5 | 5 | 5 | 3 |
| Research grounding | **5** (cited, incl. verified Opus anchors) | 3.5 (real, uncited) | 4.5 (cited) | 2 |
| PRD rigor | **5** | 5 | 4.5 | 3 |
| Data model | **5** | 5 | 4.5 | 5 |
| Flows | **5** | 5 | 4 | 5 |
| Design system | **5** | 5 | 4.5 | 4 |
| Infra / GDPR | **5** | 4.5 | 5 | 2 |
| Planning | **5** | 5 | 4.5 | 4 |
| Internal consistency | **5** | 4.5 | 5 | 4 |
| Signal density | **5** | 5 | 5 | 4 |

\* Combined is scored by its own authoring process — by construction it adopts each dimension's best-of-breed technique, so read the column as "designed to max the rubric" and weigh the honest caveats in the verdict.

### Verdict

- **Combined** takes the strongest treatment of every dimension *and* fixed eight real defects that no single source suite had caught: a consent-history data gap (new `consent_events` table), a licensing rule that made SAUM-issued non-ULM documents unrecordable, a missing renewal carve-out in the status engine, a daily-job action list that omitted the onboarding send, three WCAG contrast failures in the inherited palette, and an inherited backup claim exceeding Supabase Pro's actual 7-day retention. Caveats: fewer diagrams than Opus (21 vs 57), a deliberate 12-of-64 wireframe selection versus Codex, and it is the newest text with the least human review.
- **Opus** wins on raw *specification craft* (GiST constraints, ~60 flow edge cases, 57 diagrams) and deep unsourced aviation-regulatory research — but invented its own pricing, cites nothing, and left GDPR retention unresolved.
- **Codex** wins on *volume and breadth* (64 wireframes, biggest requirement sweep) — but never decides prices/currency/jurisdiction and duplicates ~20–25% of itself.
- **Fable** wins among the originals on *verification and brief-fidelity* (53 cited sources, current stack, delivered-logo integration) — at 2–3× less artifact detail than Opus.

One-line summary: **Opus specified the most, Codex wrote the most, Fable verified the most — Combined merges all three, on-brief, and repaired what synthesis exposed.**

## Adding pages

1. Drop a `.md` file into `docs/Combined/`, `docs/Opus/`, `docs/Fable/`, or `docs/Codex/`.
2. Link it in [`docs/_sidebar.md`](_sidebar.md) under the matching section.
3. Commit and push — it's live.
