---
name: using-fluffyui
description: |
  Use this skill when building Go terminal UIs with FluffyUI widgets, layouts,
  FSS styling, reactive signals, accessibility, web backend, MCP agent tools,
  GPU rendering, or FluffyUI scaffolded apps.
---

# Using FluffyUI

## When to reach for FluffyUI

Use FluffyUI for rich Go TUIs: widget-heavy terminal apps, CSS-like terminal styling, reactive state, accessibility, web-backed terminal sessions, MCP-controllable interfaces, dashboards, editors, and agent-visible tools.

Use GoSX for web application UIs. Use FluffyUI when the primary surface is terminal/TUI or TUI-in-browser.

## What FluffyUI is

FluffyUI is a Go TUI framework with a large widget catalog, FSS stylesheets, flex/grid layout, reactive `Signal[T]` state, accessibility metadata, optional TTS, web transport, GPU rendering backends, and MCP tools for AI agents to read and control the interface.

## Quick start

```bash
go get github.com/odvcencio/fluffyui@latest
go install github.com/odvcencio/fluffyui/cmd/fluffy@latest
fluffy create my-app --template full
cd my-app && go run .
```

Minimal app:

```go
package main

import "github.com/odvcencio/fluffyui/fluffy"

func main() {
	app, _ := fluffy.NewApp(
		fluffy.WithRoot(fluffy.VStack(
			fluffy.Label("Hello, FluffyUI!"),
			fluffy.PrimaryButton("Click me", func() {}),
		)),
	)
	app.Run()
}
```

Enable agent control:

```go
srv, _ := agent.NewServer(agent.ServerOptions{
	Addr: "unix:/tmp/fluffyui.sock",
	App:  app,
})
go srv.Serve(ctx)
```

## When NOT to use FluffyUI

Do not use FluffyUI for a plain CLI with no persistent interface. Do not use it as a replacement for GoSX when the product is a normal web app. Do not assume every terminal supports every graphics or GPU backend.

Do not skip accessibility metadata on custom widgets; FluffyUI's built-in widgets are designed around it.

## Going deeper

- Public source: [odvcencio/FluffyUI](https://github.com/odvcencio/FluffyUI)
- `docs/getting-started.md`: Read for first app setup.
- `docs/widgets/overview.md`: Read for widget selection.
- `docs/tutorials/04-ai-agent-integration.md`: Read for MCP integration.
- `docs/integration-guide.md`: Read for custom event loops or embedding widgets.
