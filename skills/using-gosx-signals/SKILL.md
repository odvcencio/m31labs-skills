---
name: using-gosx-signals
description: |
  Use this skill when working with GoSX reactive state: signal.Signal[T],
  signal.Derive, computed values, signal.Watch effects, subscriptions,
  signal.Batch, island state, or shared $ signals across islands and engines.
---

# Using GoSX Signals

## When to reach for GoSX signals

Use signals for state that drives reactive UI or engine behavior. In islands, signals are the state cells the VM subscribes to. In server-side or engine code, signals can model derived state and effects without tying logic to the DOM.

Use ordinary Go variables for request-local values that do not need subscriptions.

## What GoSX signals are

`signal.Signal[T]` is mutable reactive state. `signal.Derive` creates a computed value by tracking signal reads. `signal.Watch` runs an effect when dependencies change. `signal.Batch` coalesces updates and fires notifications after the outermost batch returns.

Shared signal names begin with `$` in island/bridge contexts. A shared signal lets multiple islands or shared engines observe the same browser-side state.

## Quick start

Use signals directly in Go:

```go
package app

import "github.com/odvcencio/gosx/signal"

func CounterState() (*signal.Signal[int], *signal.Computed[int]) {
    count := signal.New(0)
    doubled := signal.Derive(func() int {
        return count.Get() * 2
    })
    count.Update(func(n int) int {
        return n + 1
    })
    return count, doubled
}
```

Batch related writes:

```go
signal.Batch(func() {
    firstName.Set("Ada")
    lastName.Set("Lovelace")
})
```

In `.gsx` islands, declare state inside the island component and bind it into text, attributes, and handlers.

## When NOT to use GoSX signals

Do not use signals for database persistence, session state, or cross-request server state without a real storage model.

Do not use effects for DOM mutation in ordinary island templates. Prefer declarative text and attribute bindings; the island VM patches the DOM.

Do not share every signal. Use `$` shared signals only when multiple islands or engines need the same page-level state.

Do not expect arbitrary goroutines, channels, or server-only behavior to run inside islands.

## Going deeper

- `signal.New(initial)`: mutable state.
- `signal.NewWithEqual(initial, eq)`: custom equality.
- `signal.Derive(fn)`: computed value with dependency tracking.
- `signal.Watch(fn)`: effect with disposal.
- `signal.Batch(fn)`: coalesce notifications.
- Use `using-gosx-islands` for browser hydration constraints.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
