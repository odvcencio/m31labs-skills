---
name: using-gosx-simulation
description: |
  Use this skill when building GoSX simulations, games, fixed-step loops,
  physics worlds, server-authoritative ticks, input mapping, replay,
  spectators, volumetric fields, particle advection, field streaming, Scene3D
  mounted simulations, or game/runtime orchestration.
---

# Using GoSX Simulation

## When to reach for GoSX simulation

Use GoSX simulation packages when a page needs deterministic time, physics, repeated input, server-authoritative state, volumetric data, replay, or Scene3D-mounted interactive runtime. This includes games, scientific visualization, live simulation dashboards, and collaborative physical scenes.

Use a normal island for small UI interactions. Use Scene3D alone for static or mostly visual 3D scenes. Add `game`, `sim`, `physics`, and `field` only when runtime state evolves over ticks.

## What GoSX simulation is

GoSX simulation is a set of pure-Go packages:

- `sim`: server-authoritative fixed-rate simulation over hubs.
- `game`: runtime orchestration with loop phases, input, assets, ECS-style state, Scene3D mounting, audio, and sim integration.
- `physics`: rigid bodies, colliders, constraints, raycasts, CCD, Scene3D bridge.
- `field`: 3D vector fields, sampling, operators, quantization, and hub streaming.

## Quick start

Implement the `sim.Simulation` contract and run it through a hub:

```go
package app

import (
    "github.com/odvcencio/gosx/hub"
    "github.com/odvcencio/gosx/sim"
)

type Match struct {
    frame int
}

func (m *Match) Tick(inputs map[string]sim.Input) {
    m.frame++
}

func (m *Match) Snapshot() []byte { return []byte(`{"frame":0}`) }
func (m *Match) Restore(snapshot []byte) {}
func (m *Match) State() []byte { return []byte(`{"running":true}`) }

func StartMatch() *sim.Runner {
    h := hub.New("match")
    runner := sim.New(h, &Match{}, sim.Options{TickRate: 60, StateEncoding: sim.StateEncodingJSON})
    runner.RegisterHandlers()
    runner.Start()
    return runner
}
```

Use `field` for volumetric data:

```go
wind := field.FromFunc([3]int{32, 32, 32}, 3, bounds, func(x, y, z float32) []float32 {
    return []float32{-y, x, 0}
})
quantized := wind.Quantize(field.QuantizeOptions{BitWidth: 6})
decoded := quantized.Decompress()
```

## When NOT to use GoSX simulation

Do not use a server-authoritative loop for ordinary forms, dashboards, or CRUD pages.

Do not put the authoritative game state in an island if fairness, replay, or spectator sync matters.

Do not ship volumetric fields without quantization or an explicit bandwidth plan.

Do not assume physics is required for every Scene3D app. Static scenes and model viewers usually do not need it.

## Going deeper

- `sim.Runner`: fixed tick rate, input collection, state broadcast, snapshots, replay, spectator sync.
- `game.Runtime`: loop phases, input actions, assets, Scene3D mounting, audio, and ECS-style state.
- `physics.World`: bodies, colliders, constraints, raycasts, conservative CCD.
- `field`: `Advect`, `Curl`, `Divergence`, `Gradient`, `Blur`, `Resample`, `PublishField`, `SubscribeField`.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
