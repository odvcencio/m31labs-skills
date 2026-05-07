---
name: using-ferrous-wheel
description: |
  Use this skill when writing or maintaining Ferrous Wheel .fw files,
  transpiling Rust-inspired Go syntax, running .fw scripts, formatting or
  linting .fw source, using Ferrous Wheel LSP, or reviewing generated Go.
---

# Using Ferrous Wheel

## When to reach for Ferrous Wheel

Use Ferrous Wheel when Go code benefits from Rust-inspired syntax sugar but must compile to standard Go: generic enums, `derive`, `impl`, `Result`/`Option`, expression-oriented helpers, pipelines, low-level memory primitives, or `.fw` scripts.

It is useful for build/training scripts and application code where concise `.fw` source is preferred but the runtime artifact should remain ordinary Go.

## What Ferrous Wheel is

Ferrous Wheel is a `.fw` to Go transpiler built on gotreesitter grammargen. It emits standard Go with no runtime library or CGo dependency. The CLI can run, build, emit, format, lint, and serve an LSP for `.fw` source.

## Quick start

```bash
go install github.com/odvcencio/ferrous-wheel/cmd/ferrous-wheel@latest
```

```bash
ferrous-wheel emit myfile.fw
ferrous-wheel run myfile.fw
ferrous-wheel build myfile.fw -o dist/myapp
ferrous-wheel fmt -w myfile.fw
ferrous-wheel lint myfile.fw
ferrous-wheel lsp
```

Example:

```fw
enum Color { Red, Green, Blue(int) }

derive Stringer for Color

func main() {
    let color = Blue(3)
    fmt.Println(color)
}
```

## When NOT to use Ferrous Wheel

Do not introduce `.fw` into a repo that expects plain Go unless the user or project already chose Ferrous Wheel. Do not hand-edit generated Go and then treat it as the source of truth; edit the `.fw` source.

Do not skip `ferrous-wheel lint`; lint errors block transpilation and warnings often catch source-level mistakes.

## Going deeper

- Public source: [odvcencio/ferrous-wheel](https://github.com/odvcencio/ferrous-wheel)
- README "Usage": Read for CLI command shapes.
- README language sections: Read for `enum`, `derive`, `impl`, `Result`, `Option`, and memory primitives.
- VS Code Marketplace link in the README: Use for editor setup.
