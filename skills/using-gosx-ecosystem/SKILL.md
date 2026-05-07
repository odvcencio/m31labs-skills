---
name: using-gosx-ecosystem
description: |
  Use this skill when working with GoSX ecosystem packages outside the core
  web framework: gosx-native, gosx-admin, gosx-cms, the optional gosx/editor
  module, native iOS/Android generation, CMS block catalogs, admin block
  editing, Markdown++ editor shells, or deciding whether a peripheral package
  belongs in an app.
---

# Using The GoSX Ecosystem

## When to reach for GoSX ecosystem packages

Reach for the ecosystem packages when core GoSX primitives are not the whole job:

- `gosx-native`: generate native iOS SwiftUI and Android Compose surfaces from GoSX/NIR.
- `github.com/odvcencio/gosx/editor`: mount a Markdown++ editor shell inside a GoSX app.
- `gosx-admin`: build typed admin surfaces, block editors, and reusable admin primitives.
- `gosx-cms`: use opinionated CMS block catalogs and content patterns on top of `gosx-admin`.

Use core GoSX first for ordinary web routing, actions, islands, hubs, engines, and Scene3D. Add ecosystem packages only when the app needs their higher-level product surface.

## What these packages are

`gosx-native` is the mobile counterpart to GoSX. It lowers GoSX source through shared NIR and emits deterministic SwiftUI/Compose source, app support declarations, native route/data/action clients, and native Scene3D surfaces.

`gosx/editor` is an optional module with its own `go.mod`. It renders a Markdown++ editor shell and serves browser assets for preview, autosave, outline, gallery, metadata, toolbar, text operations, and Markdown-oriented editing. It intentionally does not own Markdown++ rendering; apps import `mdpp` directly.

`gosx-admin` is the generic admin layer. Its current public package is `blockstudio`, which defines sortable block catalogs, normalized persisted order, form view models, and an embedded browser runtime for block lists.

`gosx-cms` is the opinionated CMS layer. It builds reusable CMS patterns on top of `gosx-admin`; its current public package is `blocks`, including a `HomeCatalog` block catalog.

## Quick start

Use `gosx-native` for native source generation:

```bash
go run ./cmd/gsxnative init /tmp/MyApp --name MyApp --module com.example.myapp
cd /tmp/MyApp
gsxnative build all --codegen-only
```

Mount the editor only in apps that need it:

```go
app.Mount("/editor/", http.StripPrefix("/editor/", editor.AssetHandler()))

ed := editor.New("post-editor", editor.Options{
    Content:     post.Content,
    Title:       post.Title,
    FormAction:  ctx.ActionPath("update"),
    AutoSaveURL: ctx.ActionPath("autosave"),
    PreviewURL:  ctx.ActionPath("preview"),
    CSRFToken:   token,
})
return ed.Render()
```

Use block catalogs through admin/CMS packages:

```go
catalog := blocks.HomeCatalog()
enabled := blockstudio.Normalize(savedBlocks, catalog)
formBlocks := blockstudio.FormBlocks(enabled, catalog, blockstudio.FormOptions{Prefix: "home"})
_ = formBlocks
```

## When NOT to use ecosystem packages

Do not use `gosx-native` for responsive web output. Use core GoSX web builds unless the target is native iOS or Android.

Do not treat `gosx-native` as fully production-complete native app infrastructure yet. Source-owned declarations, typed codecs, auth/session plumbing, release packaging, and full native Scene3D parity are still active readiness gaps.

Do not use `gosx/editor` as the Markdown++ renderer. Keep preview/render endpoints application-owned and import `github.com/odvcencio/mdpp` directly.

Do not use `gosx-admin` or `gosx-cms` for one-off CRUD pages if a plain GoSX route plus actions is simpler.

Do not put app-specific CMS schema into `gosx-admin`. `gosx-admin` owns generic admin primitives; `gosx-cms` owns opinionated content patterns.

## Going deeper

- `gosx-native`: `gsxnative init`, `build`, `emit`, `check`, and `scene-conform`.
- `gosx/editor`: `editor.New`, `Options`, `AssetHandler`, toolbar actions, textmodel operations, editor panels.
- `gosx-admin/blockstudio`: `Definition`, `FieldDefinition`, `Normalize`, `InputsFromBlocks`, `FormBlocks`, `RuntimeHandler`.
- `gosx-cms/blocks`: reusable CMS block catalogs, currently including `HomeCatalog`.
- Public sources: [odvcencio/gosx](https://github.com/odvcencio/gosx), [odvcencio/gosx-native](https://github.com/odvcencio/gosx-native), [odvcencio/gosx-admin](https://github.com/odvcencio/gosx-admin), [odvcencio/gosx-cms](https://github.com/odvcencio/gosx-cms)
