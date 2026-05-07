---
name: using-gosx-build
description: |
  Use this skill when working with the GoSX CLI, project scaffolding, dev
  server, hot reload, compile/check/render/fmt, production builds, static
  export, TinyGo runtime output, performance budgets, visual regression,
  desktop packaging, offline bundles, MSIX, assets planning, or route runtime
  size inspection.
---

# Using GoSX Build

## When to reach for GoSX build tooling

Use the GoSX CLI whenever the user needs to scaffold, inspect, compile, run, build, export, profile, or package a GoSX app. Prefer the CLI over guessing generated file names or runtime assets.

Use `gsxnative` from `gosx-native` when the target is native iOS/Android generation rather than web, desktop, or static/export output.

## What GoSX build tooling is

`gosx` is the command surface for the framework. It can initialize apps, run a dev server, compile `.gsx`, validate and format source, build deployable bundles, export static pages, profile performance, run visual diffs, inspect runtime sizes, and host desktop apps.

## Quick start

Core commands:

```bash
gosx init my-app
gosx dev my-app
gosx check my-app/app/page.gsx
gosx compile my-app/app/page.gsx
gosx fmt my-app/app
gosx build --prod my-app
gosx export my-app
```

Performance and size:

```bash
gosx perf --json --budget perf-budget.json http://localhost:3000/
gosx perf budget perf.json perf-budget.json
gosx size --json dist
gosx visual --update http://localhost:3000/
```

Ask the installed CLI for exact command shape:

```bash
gosx help
gosx build --help
gosx assets plan --help
```

## When NOT to use GoSX build tooling

Do not hand-edit generated runtime bundles as the normal workflow. Change source and rebuild.

Do not use production build assumptions in dev mode. Dev uses standard-Go WASM for local iteration; production builds require TinyGo for browser runtime output.

Do not skip `gosx perf` and size inspection when adding islands, engines, hubs, Scene3D, or managed media.

Do not use desktop packaging as a reason to fork app code. Treat desktop as a build/host target unless native bridge behavior is actually required.

## Going deeper

- `gosx init [dir] [--template docs]`: scaffold an app.
- `gosx dev <dir>`: hot reload development server.
- `gosx build [--prod|--offline|--msix|--sign] <dir>`: production and packaging output.
- `gosx export <dir>`: pre-render static pages.
- `gosx perf`, `gosx visual`, `gosx size`: performance, screenshots, and runtime footprint.
- `gosx desktop`: native desktop host.
- `gosx assets plan`: Scene3D/game asset planning.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
