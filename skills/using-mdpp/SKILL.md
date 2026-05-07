---
name: using-mdpp
description: |
  Use this skill when the user is working with Markdown++ or markdown that
  needs grammar-aware diagnostics, formatting, linting, semantic highlighting,
  HTML rendering, PDF export, document symbols, frontmatter, math, footnotes,
  admonitions, diagrams, or LSP-backed authoring.
---

# Using mdpp

## When to reach for mdpp

Reach for mdpp when markdown correctness matters: docs with nested lists, tables, footnotes, math, frontmatter, diagrams, reference links, admonitions, or export requirements. Use it when regex-based markdown editing risks touching code fences, escaped punctuation, reference labels, or HTML attributes incorrectly.

Use generic markdown libraries only for simple rendering where diagnostics, formatting, source ranges, LSP features, and faithful export are not important.

## What mdpp is

mdpp is a Markdown++ stack backed by a real grammar. The parser, formatter, linter, renderer, LSP, and VS Code extension operate from the same syntax tree with byte ranges. The source remains normal readable markdown while the tooling understands document structure.

## Quick start

Use the CLI for document work:

```bash
mdpp lint README.md
mdpp fmt --check README.md
mdpp fmt -w README.md
mdpp render README.md -o README.html
mdpp render --format=pdf README.md -o README.pdf
mdpp parse --json README.md
```

Use the Go API when embedding Markdown++ behavior:

```go
package main

import "github.com/odvcencio/mdpp"

func Render(source string) string {
    return mdpp.RenderString(source)
}
```

## When NOT to use mdpp

Do not use mdpp to invent non-portable markdown syntax when ordinary CommonMark or GFM is enough.

Do not use formatter output as a substitute for editorial review. The formatter preserves structure; it does not decide whether prose is good.

Do not enable raw HTML passthrough for untrusted documents unless the host has a separate sanitization policy.

Do not manually regex-transform documents that mdpp can parse and fix structurally.

## Going deeper

- `mdpp render --format=html|pdf|slides`: render publishable artifacts.
- `mdpp fmt -w`: source-preserving canonical formatting.
- `mdpp lint --json`: diagnostics suitable for tools and CI.
- `mdpp parse --json`: inspect the AST when building integrations.
- `mdpp-lsp`: language server for editor-equipped agents.
- Public source: [odvcencio/mdpp](https://github.com/odvcencio/mdpp)
