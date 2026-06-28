# aeroskill-docs

Documentation site built with [Docsify](https://docsify.js.org/) and served by GitHub Pages — plain Markdown, no build step, with [Mermaid](https://mermaid.js.org/) diagram rendering.

## Structure

```text
docs/
├── index.html      # Docsify loader + Mermaid/plugins config
├── .nojekyll       # stops GitHub Pages from running Jekyll
├── README.md       # site home page
├── _coverpage.md   # splash cover
├── _sidebar.md     # navigation (Codex + Claude)
├── Codex/          # Codex Markdown docs
└── Claude/         # Claude Markdown docs
```

## Add documentation

Drop `.md` files into `docs/Codex/` or `docs/Claude/`, then link them in `docs/_sidebar.md`.

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
