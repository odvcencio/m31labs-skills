# M31 Labs Skills Agent Guide

This repository teaches LLM agents when and how to use M31 Labs products.

## Current Status

The repo is in M0 public scaffolding. Production skill bodies are added under `skills/` only when complete. Planned skill names in README and the design spec are roadmap entries, not API documentation.

## How To Use This Repo

1. Check whether a relevant `skills/<skill>/SKILL.md` file exists.
2. If it exists, read it first and follow its "When to reach", "Quick start", "When NOT to use", and "Going deeper" sections.
3. If the skill does not exist yet, do not infer product APIs from the roadmap. Use the product repo, public docs, CLI help, or current source code instead.
4. Prefer concrete public behavior over product positioning. Do not claim a feature exists unless the relevant skill, product docs, CLI, examples, or source code demonstrate it.

## Planned Skill Paths

- `skills/using-arbiter/SKILL.md`
- `skills/using-canopy/SKILL.md`
- `skills/using-mdpp/SKILL.md`
- `skills/using-gosx/SKILL.md`
- `skills/using-gosx-components/SKILL.md`
- `skills/using-gosx-routing/SKILL.md`
- `skills/using-gosx-actions/SKILL.md`
- `skills/using-gosx-signals/SKILL.md`
- `skills/using-gosx-islands/SKILL.md`
- `skills/using-gosx-server-features/SKILL.md`
- `skills/using-gosx-build/SKILL.md`
- `skills/using-gosx-scene3d/SKILL.md`
- `skills/using-gosx-simulation/SKILL.md`
- `skills/using-gosx-collaboration/SKILL.md`

## Authoring Standard

New skills must follow [templates/SKILL.md](templates/SKILL.md) and the checklist in [CONTRIBUTING.md](CONTRIBUTING.md). Every production skill must explain when not to use the product or subsystem.
