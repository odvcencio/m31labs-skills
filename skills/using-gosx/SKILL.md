---
name: using-gosx
description: |
  Use this skill for general GoSX adoption, project setup, architecture choices,
  deciding between server components, actions, islands, engines, and hubs, or
  routing a GoSX question to a more focused GoSX skill.
---

# Using GoSX

## When to reach for GoSX

Reach for GoSX when the user wants a Go-native web application platform: server-rendered pages by default, `.gsx` components, optional interactive islands through WebAssembly, typed server actions, file routing, hubs, engines, Scene3D, simulation, and collaboration primitives without a JavaScript app toolchain.

Use the focused GoSX skills by task:

- Components and `.gsx`: `using-gosx-components`.
- File routes and loaders: `using-gosx-routing`.
- Mutations and forms: `using-gosx-actions`.
- Reactive state: `using-gosx-signals`.
- Client hydration: `using-gosx-islands`.
- Auth, sessions, caching, streaming, media: `using-gosx-server-features`.
- CLI, dev, build, perf, desktop, export: `using-gosx-build`.
- 3D scenes: `using-gosx-scene3d`.
- Games, fields, physics, simulation: `using-gosx-simulation`.
- Hubs, CRDTs, workspace sync: `using-gosx-collaboration`.
- Native mobile, editor, admin, and CMS periphery: `using-gosx-ecosystem`.

## What GoSX is

GoSX treats the browser as a render target, not the primary runtime. Server components are Go functions that render HTML. Islands opt into browser interactivity and compile constrained Go expressions into a shared WASM VM. Engines handle heavy client work like canvas, WebGL, WebGPU, workers, video, and games. Hubs provide WebSocket presence, fanout, and shared state.

The five execution primitives are: Server, Action, Island, Engine, and Hub. Do not blur them. A static page should stay Server-only. A form should use an Action. A counter or small reactive DOM area should be an Island. A 3D surface or game should use an Engine. Shared realtime state should use a Hub.

## Quick start

Scaffold and run an app:

```bash
gosx init my-app
cd my-app
go run .
```

Or start from a minimal server:

```go
package main

import (
    "net/http"

    "github.com/odvcencio/gosx"
    "github.com/odvcencio/gosx/server"
)

func main() {
    app := server.New()
    app.Page("/", func(r *http.Request) gosx.Node {
        return gosx.El("h1", gosx.Text("Hello from GoSX"))
    })
    app.ListenAndServe(":8080")
}
```

## When NOT to use GoSX

Do not choose GoSX when the team needs the React/npm ecosystem as the primary design constraint.

Do not make every component an island. Server components are the baseline; islands are for interactive DOM state.

Do not put arbitrary Go runtime behavior inside islands. Islands compile a constrained browser expression subset.

Do not use Scene3D, game, workspace, or semantic packages for ordinary CRUD pages unless the page actually needs those capabilities.

Do not pull in `gosx-native`, `gosx-admin`, `gosx-cms`, or `gosx/editor` by default. They are opt-in ecosystem packages for native targets, admin/CMS surfaces, and in-app Markdown++ editing.

## Going deeper

- Run `gosx help` and `gosx <command> --help` for exact CLI shape.
- Start with server components and file routing, then add actions, signals, islands, and build/perf tooling.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
