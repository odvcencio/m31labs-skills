---
name: using-gosx-scene3d
description: |
  Use this skill when building GoSX Scene3D experiences: typed scene graphs,
  meshes, materials, PBR, cameras, lights, shadows, glTF/GLB models,
  animations, post-processing, WebGPU/WebGL fallback, asset planning,
  server-driven scene diffs, picking, labels, sprites, or 3D performance.
---

# Using GoSX Scene3D

## When to reach for Scene3D

Use Scene3D for 3D surfaces inside GoSX apps: product viewers, simulation visualization, scientific scenes, dashboards with spatial data, game visuals, glTF/GLB model display, particles, or server-driven scene updates.

Use ordinary DOM/CSS for two-dimensional UI. Use a custom engine only when Scene3D's typed scene graph is the wrong abstraction.

## What Scene3D is

Scene3D is a Go-authored 3D engine. You describe scenes as typed Go values; GoSX lowers them to SceneIR and renders through WebGPU and WebGL-capable runtime paths. There is no three.js scene graph. SceneIR is shared across backends and supports graceful capability fallback.

## Quick start

Build a typed scene:

```go
package app

import "github.com/odvcencio/gosx/scene"

func ProductScene() scene.Props {
    return scene.Props{
        Responsive: scene.Bool(true),
        Controls:   "orbit",
        Camera: scene.PerspectiveCamera{
            Position: scene.Vec3(0, 1.5, 5),
            FOV:      60,
        },
        Graph: scene.NewGraph(
            scene.DirectionalLight{
                Color: "#fff1d6",
                Intensity: 1.0,
                Direction: scene.Vec3(0.3, -1, -0.5),
                CastShadow: true,
            },
            scene.Mesh{
                Geometry: scene.SphereGeometry{Segments: 32},
                Material: scene.StandardMaterial{Color: "#D4AF37", Roughness: 0.3, Metalness: 0.9},
                Position: scene.Vec3(0, 0.5, 0),
                CastShadow: true,
                ReceiveShadow: true,
            },
        ),
    }
}
```

Plan assets before shipping heavy scenes:

```bash
gosx assets plan public
gosx perf --budget perf-budget.json http://localhost:3000/scene
```

## When NOT to use Scene3D

Do not use Scene3D for decorative effects that CSS can handle.

Do not require WebGPU unless the scene truly needs it. Let capability fallback cover WebGPU to WebGL to canvas where possible.

Do not ship large GLB, texture, HDR, or animation assets without running asset planning and performance budgets.

Do not mutate scenes by regenerating full pages when a compact scene diff or hub event is the right runtime shape.

## Going deeper

- Graph nodes include groups, meshes, instancing, points, labels, sprites, models, lights, helpers, LOD, and particles.
- Materials include PBR standard, flat, glass, glow, ghost, matte, line, and custom shader hooks.
- Cameras include perspective/orthographic plus orbit, fly, first-person, drag, pointer lock, picking, and transition hints.
- Use `scene.DiffCommands(previous.SceneIR(), next.SceneIR())` for server-driven updates.
- Use `using-gosx-simulation` when Scene3D is the visual layer for a game or server-authoritative simulation.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
