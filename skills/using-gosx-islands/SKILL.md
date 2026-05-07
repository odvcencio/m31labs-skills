---
name: using-gosx-islands
description: |
  Use this skill when implementing GoSX island components, //gosx:island
  hydration, island manifests, event handlers, signal-backed DOM updates,
  shared $ signals, WASM runtime selection, client VM constraints, or
  debugging why a component should stay server-rendered.
---

# Using GoSX Islands

## When to reach for GoSX islands

Use an island for a small interactive DOM subtree: counters, filters, toggles, tabs, local forms, controls around an engine, or UI that reacts to page-level shared state.

Keep the rest of the page as server-rendered HTML. A GoSX page should pay island runtime cost only where interaction requires it.

## What GoSX islands are

An island is a `.gsx` component marked with `//gosx:island`. The compiler extracts signals, computed values, handlers, and supported expressions, then serializes a compact island program. The browser hydrates that program through a shared WASM VM and patch runtime.

Island expressions are constrained. Literals, props, signal access, arithmetic, comparisons, boolean logic, string operations, conditionals, handler dispatch, and list iteration are the target shape. Arbitrary Go, goroutines, channels, filesystem calls, and server APIs are not island code.

## Quick start

Create an island component:

```go
package app

//gosx:island
func Counter() Node {
    count := signal.New(0)
    return <div class="counter">
        <button data-on-click="count.Set(count.Get() - 1)">-</button>
        <span>{count.Get()}</span>
        <button data-on-click="count.Set(count.Get() + 1)">+</button>
    </div>
}
```

Validate the file:

```bash
gosx check app/counter.gsx
gosx compile app/counter.gsx
```

Use shared state only when more than one island needs it:

```go
theme := signal.NewShared("$theme", "dark")
```

## When NOT to use GoSX islands

Do not mark server-only components as islands. Static HTML, data display, and initial page rendering should stay server-side.

Do not put secrets, database access, network credentials, filesystem behavior, goroutines, or server-only packages in island code.

Do not use islands for heavy rendering loops. Use an engine or Scene3D for canvas, WebGL, WebGPU, or game surfaces.

Do not create many tiny islands that all duplicate state. Prefer one coherent island or shared `$` state when interaction is coupled.

## Going deeper

- Use `using-gosx-signals` for signal semantics.
- Use `using-gosx-actions` when interactivity submits a server mutation.
- Use `using-gosx-scene3d` or `using-gosx-simulation` for engine surfaces.
- Inspect build output and `dist/export.json` to confirm which routes actually ship islands and runtime assets.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
