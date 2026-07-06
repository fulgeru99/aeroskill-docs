# aeroskill-docs

Documentation site built with [Docsify](https://docsify.js.org/) and served by GitHub Pages — plain Markdown, no build step, with [Mermaid](https://mermaid.js.org/) diagram rendering.

Three independent, complete specification suites for the **Aeroskill Club** general-aviation membership platform: **Opus** (formerly "Claude"), **Fable**, and **Codex**.

## Structure

```text
docs/
├── index.html      # Docsify loader + Mermaid/plugins config
├── .nojekyll       # stops GitHub Pages from running Jekyll
├── README.md       # site home page (includes the suite comparison)
├── _coverpage.md   # splash cover
├── _sidebar.md     # navigation (Opus · Fable · Codex)
├── Opus/           # Opus spec suite (formerly Claude/)
├── Fable/          # Fable spec suite
└── Codex/          # Codex spec suite
```

## The three suites, compared

A cross-review (July 2026) profiled all three suites against the same 10-dimension rubric — the full comparison with scorecards lives on the [site home page](docs/README.md). The short version:

| | Opus | Fable | Codex |
|---|---|---|---|
| Size | 6,489 lines | 2,614 lines | 11,188 lines |
| Tier pricing | Own model: free / 490 / 1.490 RON (concept figures) | **Per brief: 3000 / 4500 / 6000 RON/yr, locked** | Deferred — never priced |
| Requirements | **114 with IDs + Given/When/Then** | 86 with IDs + criteria | ~250–300, no IDs |
| Research citations | ~0 (real but uncited facts) | **53 links, 26 domains** (laws, market, fees, standards) | 11 (vendor docs) |
| Database tables | **~43** | 20 (+ SQL RLS sketches) | 29 |
| Diagrams | **57** | 13 | 9 |
| Distinctive strength | Deepest per-artifact spec craft | Verified research, brief-fidelity, consistency | Wireframes (64) + breadth |

One-line summary: **Opus specified the most, Codex wrote the most, Fable verified the most.**

## Add documentation

Drop `.md` files into `docs/Opus/`, `docs/Fable/`, or `docs/Codex/`, then link them in `docs/_sidebar.md`.

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
