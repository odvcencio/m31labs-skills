---
name: using-gosx-components
description: |
  Use this skill when writing or reviewing GoSX `.gsx` components, component
  props, Node API usage, element/text/fragment creation, markup expressions,
  attributes, slots, layouts, CSS scoping, or server-vs-island component shape.
---

# Using GoSX Components

## When to reach for GoSX components

Use GoSX components for HTML UI authored in Go and `.gsx`. Server components return `Node` and render to HTML. Island components also return `Node`, but add `//gosx:island` and must follow the island subset.

Use `.gsx` when markup readability matters. Use the `gosx.El`, `gosx.Text`, `gosx.Attrs`, `gosx.Attr`, `gosx.Fragment`, and `gosx.RawHTML` Node API when generating small nodes directly from Go.

## What GoSX components are

A component is a Go function that returns `Node`. `.gsx` extends Go with embedded markup at the grammar level. Components are not string templates; they compile through the GoSX parser, lower to IR, validate, and render through the framework.

## Quick start

Create `app/page.gsx`:

```go
package app

func Page() Node {
    return <main class="page">
        <h1>Hello from GoSX</h1>
        <p>{data.greeting}</p>
    </main>
}
```

Direct Node API equivalent:

```go
package app

import "github.com/odvcencio/gosx"

func Title(text string) gosx.Node {
    return gosx.El("h1", gosx.Attrs(gosx.Attr("class", "title")), gosx.Text(text))
}
```

Validate component syntax:

```bash
gosx check app/page.gsx
gosx render app/page.gsx
gosx fmt app/page.gsx
```

## When NOT to use GoSX components

Do not use `.gsx` for non-UI business logic. Keep data access and mutations in Go server modules, handlers, or packages.

Do not use `gosx.RawHTML` for untrusted content. Prefer structured nodes or a renderer with a sanitization policy.

Do not add an island directive just because a component has props. Server components can receive props and data without client hydration.

Do not create a separate skill or abstraction for CSS scoping. Use sidecar CSS, `css.ScopeCSS`, CSS layers, and ordinary classes.

## Going deeper

- Use `using-gosx-routing` for page/layout file placement and `data`.
- Use `using-gosx-islands` before adding `//gosx:island`.
- Use `using-gosx-signals` for reactive state inside islands.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
