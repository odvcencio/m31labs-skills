---
name: using-canopy
description: |
  Use this skill when the user needs structural code intelligence: finding
  symbols or references, tracing call graphs, estimating refactor blast radius,
  detecting dead code, checking architecture boundaries, analyzing complexity,
  reviewing a PR, producing RAG chunks, or running an MCP server for agents.
---

# Using Canopy

## When to reach for Canopy

Reach for Canopy before manually reading a large codebase when structure matters. Use it ahead of plain grep for questions like "where is this symbol used", "what calls this function", "what breaks if I change this", "which functions are dead", "where are the hotspots", or "does this package cross a boundary".

Use plain `rg` for literal text and small-file lookup. Use Canopy when symbol identity, AST shape, imports, call edges, complexity, generated-code filtering, or multi-language indexing changes the answer.

## What Canopy is

Canopy is a structural code analysis toolkit powered by gotreesitter. It indexes source into language-aware symbols, references, imports, call graphs, and quality signals across many languages. Its CLI is the primary interface, and `canopy mcp` exposes the same ideas to AI agents as MCP tools.

## Quick start

Start by building an index in the target repo:

```bash
canopy index build .
canopy index stats
```

Then choose the command family by question:

```bash
canopy search refs ParseConfig .
canopy graph calls EvaluateRules .
canopy graph impact EvaluateRules .
canopy graph dead ./pkg
canopy analyze hotspot .
canopy analyze check --max-cyclomatic 30
```

For unfamiliar flags, ask the installed binary:

```bash
canopy --help
canopy search refs --help
canopy graph impact --help
canopy analyze review --help
```

## When NOT to use Canopy

Do not use Canopy as a replacement for tests. It can tell you likely impact, callers, boundaries, and risk, but it does not prove runtime behavior.

Do not use structural output blindly on generated code unless generated code is the target. Canopy excludes generated files by default; use `--include-generated` only when you mean it.

Do not run repo-wide analysis when a scoped path answers the question faster. Prefer `./pkg/foo`, `path/to/file.go:Symbol`, or command filters on large repositories.

Do not use write-capable MCP or transform commands unless the user has asked for code changes and you have reviewed the intended edit.

## Going deeper

- `canopy index`: build, inspect, diff, export, validate structural indexes.
- `canopy search`: refs, symbols, imports, raw tree-sitter queries, selector grep, focused context.
- `canopy graph`: calls, impact, dead code, dependencies, fan-in, drift, test maps.
- `canopy analyze`: check, review, complexity, hotspots, boundaries, licenses, reachability, report.
- `canopy transform`: structural refactors and AST-boundary chunks.
- `canopy mcp --root .`: expose structural tools to agents; add `--allow-writes` only for trusted mutation workflows.
- Public source: [odvcencio/canopy](https://github.com/odvcencio/canopy)
- Latest release: [Canopy releases/latest](https://github.com/odvcencio/canopy/releases/latest)
