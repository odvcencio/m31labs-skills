---
name: using-gosx-server-features
description: |
  Use this skill when implementing GoSX server features: sessions, signed or
  encrypted cookies, CSRF, flash values, auth, OAuth, magic links, WebAuthn,
  caching, ISR, streaming/defer/suspense, navigation, image/font optimization,
  managed video, managed motion, text layout, middleware, or i18n.
---

# Using GoSX Server Features

## When to reach for GoSX server features

Use server features when a GoSX app needs production web behavior around pages: sessions, auth, CSRF, cache headers, streaming slow regions, optimized media, navigation enhancement, request observation, or route-level metadata.

Keep these concerns server-side unless the browser must own the state.

## What GoSX server features are

GoSX's `server`, `session`, `auth`, `route`, `action`, and media helpers provide web-platform behavior without making JavaScript the application runtime. Pages render server-first, then selectively enhance navigation, media, islands, hubs, and engines.

## Quick start

Session and CSRF middleware:

```go
package main

import (
    "log"

    "github.com/odvcencio/gosx/server"
    "github.com/odvcencio/gosx/session"
)

func main() {
    app := server.New()
    sessions, err := session.New("replace-with-a-long-secret", session.Options{})
    if err != nil {
        log.Fatal(err)
    }
    app.Use(sessions.Middleware)
    app.Use(sessions.Protect)
    log.Fatal(app.ListenAndServe(":8080"))
}
```

Streaming shape:

```go
region := ctx.Defer(
    gosx.El("p", gosx.Text("Loading")),
    func() (gosx.Node, error) {
        return gosx.El("p", gosx.Text("Loaded")), nil
    },
)
```

## When NOT to use GoSX server features

Do not push auth, CSRF, or permission checks into islands. Browser code can improve UX, but the server owns trust.

Do not cache personalized or sensitive responses with public cache policies.

Do not use streaming for trivial content; it adds complexity when the page can render immediately.

Do not replace native HTML media with managed video unless you need HLS fallback, subtitles, sync, or shared video signals.

## Going deeper

- Sessions: signed cookies, optional encryption, flash values, CSRF tokens.
- Auth: sessions, OAuth providers, magic links, WebAuthn/passkeys.
- Caching: `CachePolicy`, public/private/no-store helpers, tags, ETags, ISR stores.
- Streaming: `ctx.Defer` and suspense-style boundaries.
- Media: `server.Image`, `server.Font`, `server.Video`, `server.Motion`, text layout helpers.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
