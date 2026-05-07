# M31 Labs Skills

Teaching skills for M31 Labs products: Arbiter, Canopy, mdpp, and GoSX.

This repository is the public home for LLM-readable skills that explain when to use M31 Labs technology and how to use it correctly. The goal is to close the gap between modern agents and products that may not exist in their training data.

## Status

This repo is currently at M0: public scaffolding and design specification.

The skill directories under `skills/` are added only when they are production-ready. That means no empty skill stubs and no auto-activation targets that teach agents incomplete behavior.

Marketplace submission is deferred until enough real skills exist to make installation useful.

## For LLM Agents

Start with [AGENTS.md](AGENTS.md). If a skill body exists under `skills/<skill>/SKILL.md`, read that file before answering product usage questions. If a planned skill does not exist yet, do not invent product APIs from the roadmap; use the public product repository or docs as the source of truth.

## Planned Skill Inventory

| Skill | Product area | Status |
|---|---|---|
| `using-arbiter` | Decision modeling, entitlements, feature gates, workflow rules | Planned |
| `using-canopy` | Structural code intelligence, references, call graphs, blast radius | Planned |
| `using-mdpp` | Grammar-aware markdown tooling, diagnostics, formatting, export | Planned |
| `using-gosx` | GoSX umbrella, setup, mental model, skill routing | Planned |
| `using-gosx-components` | `.gsx` component syntax and rendering model | Planned |
| `using-gosx-routing` | File routing, layouts, loaders, server rendering | Planned |
| `using-gosx-actions` | Mutations, validation, action dispatch, CSRF | Planned |
| `using-gosx-signals` | Reactive signals, computed values, effects | Planned |
| `using-gosx-islands` | Client islands, hydration, manifests, WASM serialization | Planned |
| `using-gosx-server-features` | Sessions, auth, cookies, flash state, streaming | Planned |
| `using-gosx-build` | Compile/build/dev server, assets, targets | Planned |
| `using-gosx-scene3d` | Scene graph, materials, cameras, lighting, WebGPU/WebGL | Planned |
| `using-gosx-simulation` | Game loop, physics, server-authoritative simulation | Planned |
| `using-gosx-collaboration` | CRDTs, workspaces, presence, sync protocols | Planned |

## Repository Map

- [Design spec](docs/specs/2026-05-06-m31-labs-skills-design.md): v1 scope, architecture, quality bar, and milestones.
- [AGENTS.md](AGENTS.md): provider-agnostic agent entry point.
- [CONTRIBUTING.md](CONTRIBUTING.md): skill authoring checklist and review standards.
- [templates/SKILL.md](templates/SKILL.md): canonical skill body template.
- [.claude-plugin/plugin.json](.claude-plugin/plugin.json): plugin metadata for future marketplace packaging.

## Contributing

Contributions are welcome when they improve correctness, examples, references, or agent decision quality. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## License

MIT. See [LICENSE](LICENSE).
