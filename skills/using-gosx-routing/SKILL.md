---
name: using-gosx-routing
description: |
  Use this skill when building GoSX file-based routes, layouts, dynamic params,
  catch-all pages, route loaders, page.server.go modules, metadata, redirects,
  rewrites, cache headers, ISR, route configs, or server-rendered page setup.
---

# Using GoSX Routing

## When to reach for GoSX routing

Use GoSX routing when the user is arranging pages under `app/`, loading data for pages, nesting layouts, mapping dynamic segments, returning metadata, wiring actions to a page, or configuring cache behavior for server-rendered routes.

## What GoSX routing is

GoSX file routing discovers `page.gsx`, `layout.gsx`, `not-found.gsx`, `error.gsx`, and sibling `page.server.go` modules under an `app/` directory. The server module registers request-time hooks for a page source file: `Load`, `Metadata`, `Render`, `Actions`, and `Bindings`.

## Quick start

File layout:

```text
app/
  layout.gsx
  page.gsx
  page.server.go
  blog/
    [slug]/
      page.gsx
      page.server.go
```

Server module:

```go
package app

import (
    "log"

    "github.com/odvcencio/gosx/route"
    "github.com/odvcencio/gosx/server"
)

func init() {
    err := route.RegisterFileModuleHere(route.FileModuleOptions{
        Load: func(ctx *route.RouteContext, page route.FilePage) (any, error) {
            return map[string]string{"greeting": "Hello from the server"}, nil
        },
        Metadata: func(ctx *route.RouteContext, page route.FilePage, data any) (server.Metadata, error) {
            return server.Metadata{Title: server.Title{Absolute: "Home"}}, nil
        },
    })
    if err != nil {
        log.Fatal(err)
    }
}
```

Register routes from `main.go`:

```go
router := route.NewRouter()
err := router.AddDir(appDir, route.FileRoutesOptions{})
if err != nil {
    log.Fatal(err)
}
```

## When NOT to use GoSX routing

Do not put database calls directly in `.gsx` files. Use `page.server.go` loaders and pass data into templates.

Do not build a manual router when file routing can express the page tree cleanly.

Do not use client navigation as the baseline. Render server-first; add enhanced navigation only when the UX needs it.

Do not make catch-all routes the default for ordinary page trees. Use static and dynamic segments first.

## Going deeper

- Dynamic segment: `[slug]` gives `ctx.Param("slug")` and `params.slug`.
- Catch-all routes use the GoSX route convention from current source; verify exact naming in the product repo before adding one.
- Route modules can also expose actions and template bindings.
- Use `ctx.CachePublic`, `ctx.CacheTag`, revalidation, and route config files for cache behavior.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
