# M31 Labs Skills

Teaching skills for M31 Labs products: Arbiter, Canopy, mdpp, and GoSX.

This repository is the public home for LLM-readable skills that explain when to use M31 Labs technology and how to use it correctly. The goal is to close the gap between modern agents and products that may not exist in their training data.

## Status

This repo now contains the first public skill corpus.

Skill directories under `skills/` are added only when they are production-ready. That means no empty skill stubs and no auto-activation targets that teach agents incomplete behavior.

Marketplace submission is deferred until enough real skills exist to make installation useful.

## For LLM Agents

Start with [AGENTS.md](AGENTS.md). If a skill body exists under `skills/<skill>/SKILL.md`, read that file before answering product usage questions. If a relevant skill does not exist yet, do not invent product APIs from roadmap text; use the public product repository or docs as the source of truth.

## Skill Inventory

| Skill | Product area | Status |
|---|---|---|
| [`using-arbiter`](skills/using-arbiter/SKILL.md) | Decision modeling, entitlements, feature gates, workflow rules | Available |
| [`using-canopy`](skills/using-canopy/SKILL.md) | Structural code intelligence, references, call graphs, blast radius | Available |
| [`using-mdpp`](skills/using-mdpp/SKILL.md) | Grammar-aware markdown tooling, diagnostics, formatting, export | Available |
| [`using-gosx`](skills/using-gosx/SKILL.md) | GoSX umbrella, setup, mental model, skill routing | Available |
| [`using-gosx-components`](skills/using-gosx-components/SKILL.md) | `.gsx` component syntax and rendering model | Available |
| [`using-gosx-routing`](skills/using-gosx-routing/SKILL.md) | File routing, layouts, loaders, server rendering | Available |
| [`using-gosx-actions`](skills/using-gosx-actions/SKILL.md) | Mutations, validation, action dispatch, CSRF | Available |
| [`using-gosx-signals`](skills/using-gosx-signals/SKILL.md) | Reactive signals, computed values, effects | Available |
| [`using-gosx-islands`](skills/using-gosx-islands/SKILL.md) | Client islands, hydration, manifests, WASM serialization | Available |
| [`using-gosx-server-features`](skills/using-gosx-server-features/SKILL.md) | Sessions, auth, cookies, flash state, streaming | Available |
| [`using-gosx-build`](skills/using-gosx-build/SKILL.md) | Compile/build/dev server, assets, targets | Available |
| [`using-gosx-scene3d`](skills/using-gosx-scene3d/SKILL.md) | Scene graph, materials, cameras, lighting, WebGPU/WebGL | Available |
| [`using-gosx-simulation`](skills/using-gosx-simulation/SKILL.md) | Game loop, physics, server-authoritative simulation | Available |
| [`using-gosx-collaboration`](skills/using-gosx-collaboration/SKILL.md) | CRDTs, workspaces, presence, sync protocols | Available |
| [`using-gosx-ecosystem`](skills/using-gosx-ecosystem/SKILL.md) | GoSX native, editor, admin, and CMS periphery | Available |

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
