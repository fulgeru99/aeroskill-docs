# Mermaid Diagrams

This site renders [Mermaid](https://mermaid.js.org/) diagrams directly from fenced code blocks. Just open a code fence with `mermaid` as the language and write Mermaid syntax inside:

````markdown
```mermaid
graph TD
    A[Start] --> B{Works?}
    B -->|Yes| C[Ship it]
    B -->|No| D[Fix it]
    D --> B
```
````

…which renders as:

```mermaid
graph TD
    A[Start] --> B{Works?}
    B -->|Yes| C[Ship it]
    B -->|No| D[Fix it]
    D --> B
```

> [!TIP]
> Diagrams re-render automatically as you navigate between pages — no full reload needed.

## Flowchart

```mermaid
flowchart LR
    subgraph Client
      U[User] --> Browser
    end
    Browser -->|HTTPS| Pages[GitHub Pages]
    Pages --> MD[(Markdown files)]
    Browser -->|renders| Docsify
    Docsify --> MD
```

## Sequence diagram

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GH as GitHub
    participant Pages as GitHub Pages
    participant User
    Dev->>GH: git push (Markdown)
    GH->>Pages: deploy static files
    User->>Pages: visit site
    Pages-->>User: index.html + Markdown
    Note over User,Pages: Docsify renders in the browser
```

## Class diagram

```mermaid
classDiagram
    class Document {
      +String title
      +String[] tags
      +render() void
    }
    class Page
    class Diagram
    Document <|-- Page
    Document <|-- Diagram
    Page "1" o-- "many" Diagram
```

## State diagram

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Review: submit
    Review --> Draft: changes requested
    Review --> Published: approved
    Published --> [*]
```

## Entity relationship diagram

```mermaid
erDiagram
    REPO ||--o{ PAGE : contains
    PAGE ||--o{ DIAGRAM : embeds
    REPO {
      string name
      string branch
    }
    PAGE {
      string path
      string title
    }
```

## Gantt chart

```mermaid
gantt
    title Docs rollout
    dateFormat YYYY-MM-DD
    section Setup
    Scaffold site      :done,    a1, 2026-01-01, 3d
    Add Mermaid        :done,    a2, after a1, 2d
    section Launch
    Write guides       :active,  b1, 2026-01-06, 5d
    Publish on Pages   :         b2, after b1, 1d
```

## Pie chart

```mermaid
pie title Where docs time goes
    "Writing" : 55
    "Diagramming" : 25
    "Formatting" : 20
```

## Git graph

```mermaid
gitGraph
    commit
    branch feature
    checkout feature
    commit
    commit
    checkout main
    merge feature
    commit
```

## Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Diagram shows as raw code | The code fence language isn't exactly `mermaid` | Use ` ```mermaid ` (lowercase, no spaces) |
| All diagrams blank, console shows a Mermaid import error | The browser can't reach the Mermaid CDN | Confirm `cdn.jsdelivr.net` is reachable, or self-host `mermaid.esm.min.mjs` |
| Inline `Mermaid error:` box | Invalid Mermaid syntax in that block | Test the snippet in the [Mermaid Live Editor](https://mermaid.live) |
