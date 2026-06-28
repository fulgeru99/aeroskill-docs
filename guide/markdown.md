# Markdown & Pages

Docsify supports standard [GitHub-Flavored Markdown](https://github.github.com/gfm/) plus a few helpers. This page is itself a Markdown file — view its [source](https://github.com/fulgeru99/aeroskill-docs/blob/main/guide/markdown.md) to compare.

## Headings, text, lists

```markdown
# H1   ## H2   ### H3

**bold**, _italic_, `inline code`, ~~strikethrough~~

- bullet
  - nested
1. ordered
2. list
```

## Code blocks

Fenced code blocks get syntax highlighting (via Prism) and a copy button:

```js
function greet(name) {
  return `Hello, ${name}!`;
}
```

## Tables

| Feature | Supported |
| --- | :---: |
| Tables | ✅ |
| Footnotes | ✅ |
| Mermaid | ✅ |

## Links between pages

Link to other docs with **relative paths to the `.md` file**:

```markdown
See [Mermaid diagrams](guide/mermaid.md) and the [home page](/).
```

## Callout alerts

Docsify renders GitHub-style alerts:

> [!NOTE]
> Useful information that users should know.

> [!TIP]
> A helpful suggestion.

> [!WARNING]
> Something to watch out for.

## Images

Place images in the repo and reference them relatively:

```markdown
![Architecture](assets/architecture.png)
```

## Cover page & navigation

- **`README.md`** is the home page (`/`).
- **`_sidebar.md`** controls the left navigation.
- **`_navbar.md`** controls the top navigation.
- **`_coverpage.md`** is the splash screen.

These underscore-prefixed files are the reason the repo needs a [`.nojekyll`](guide/deploy.md) file on GitHub Pages.

Next: [add Mermaid diagrams](guide/mermaid.md).
