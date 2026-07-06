# aeroskill-docs

Documentation site built with [Docsify](https://docsify.js.org/) and served by GitHub Pages — plain Markdown, no build step, with [Mermaid](https://mermaid.js.org/) diagram rendering.

Four specification suites for the **Aeroskill Club** general-aviation membership platform: **Combined** (the synthesis — start here), plus the three independent originals it merges: **Opus** (formerly "Claude"), **Fable**, and **Codex**.

## Structure

```text
docs/
├── index.html      # Docsify loader + Mermaid/plugins config
├── .nojekyll       # stops GitHub Pages from running Jekyll
├── README.md       # site home page (includes the suite comparison)
├── _coverpage.md   # splash cover
├── _sidebar.md     # navigation (Combined · Opus · Fable · Codex)
├── Combined/       # Synthesis suite — best of the three runs, on-brief
├── Opus/           # Opus spec suite (formerly Claude/)
├── Fable/          # Fable spec suite
└── Codex/          # Codex spec suite
```

## The three suites, compared

A cross-review (July 2026) profiled the three original suites against the same 10-dimension rubric; **Combined** was then synthesized from them under explicit precedence rules (brief is law → verified research → deepest craft → breadth). The full comparison with scorecards lives on the [site home page](docs/README.md). The short version:

| | **Combined** | Opus | Fable | Codex |
|---|---|---|---|---|
| Size | 5,177 lines | 6,489 lines | 2,614 lines | 11,188 lines |
| Tier pricing | **Per brief: 3000 / 4500 / 6000 RON/yr, locked** | Own model: free / 490 / 1.490 RON (concept figures) | Per brief, locked | Deferred — never priced |
| Requirements | **111 with IDs + Given/When/Then**, + 18 flows + 32 test scenarios | 114 with IDs + Given/When/Then | 84 with IDs + criteria | ~250–300, no IDs |
| Research citations | **62 links, 29 domains** | ~0 (real but uncited facts) | 53 links, 26 domains | 11 (vendor docs) |
| Database tables | 24 (+ 47-index catalog, CRUD matrix) | **~43** | 20 (+ SQL RLS sketches) | 29 |
| Diagrams / wireframes | 21 / 12 | **57** / 0 | 13 / 0 | 9 / **64** |
| Distinctive strength | Best-of-breed synthesis, on-brief; fixed 8 defects synthesis exposed | Deepest per-artifact spec craft | Verified research, brief-fidelity | Wireframes + breadth |

One-line summary: **Opus specified the most, Codex wrote the most, Fable verified the most — Combined merges all three, on-brief, and repaired what synthesis exposed.**

## Add documentation

Drop `.md` files into `docs/Combined/`, `docs/Opus/`, `docs/Fable/`, or `docs/Codex/`, then link them in `docs/_sidebar.md`.

## Publish on GitHub Pages

**Settings → Pages → Source: _Deploy from a branch_ → Branch: `main` / Folder: `/docs`.**

The site goes live at `https://fulgeru99.github.io/aeroskill-docs/`.

> A `.github/workflows/pages.yml` workflow is also included for the optional
> _GitHub Actions_ Pages source (it uploads `./docs`). Use one source or the other,
> not both.

## Preview locally

```bash
npx docsify-cli serve docs
# or: python3 -m http.server -d docs 3000
```
