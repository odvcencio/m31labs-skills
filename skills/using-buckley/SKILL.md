---
name: using-buckley
description: |
  Use this skill when the user wants to run Buckley as an AI agent harness,
  use Buckley for commits, pull requests, reviews, plans, experiments, ACP/LSP
  surfaces, browser control, or configure governed model/tool routing.
---

# Using Buckley

## When to reach for Buckley

Use Buckley when repository work needs an agent runtime rather than a single model response: interactive coding, one-shot fixes, resumable plans, governed tool access, model routing, review passes, commits, PR authoring, ACP editor flows, or browser Mission Control.

Reach for `buckley commit` when the user explicitly asks for Buckley-authored commits or when a repo's instructions require it.

## What Buckley is

Buckley is a tool-first AI agent harness. One runtime powers TUI chat, `buckley -p`, headless sessions, browser control, ACP, and LSP. It consumes repo instructions such as `AGENTS.md`, assembles project context and skills, routes models/tools through Arbiter-style governance, and persists sessions in SQLite.

## Quick start

```bash
go install github.com/odvcencio/buckley/cmd/buckley@latest
buckley
```

Useful surfaces:

```bash
buckley -p "inspect this repo and fix the failing tests"
buckley serve --browser
buckley acp
buckley lsp
```

Commit with the Codex backend when requested:

```bash
BUCKLEY_COMMIT_BACKEND=codex buckley commit -backend codex -yes -min
```

Preview first when scope is uncertain:

```bash
BUCKLEY_COMMIT_BACKEND=codex buckley commit -backend codex -dry-run
```

## When NOT to use Buckley

Do not use Buckley just to answer a local fact that a shell command can answer directly. Do not run autonomous Buckley plans when the user asked only for analysis. Do not let Buckley stage unrelated dirty-worktree changes; stage explicit paths first and inspect `git diff --cached`.

Do not assume every repo wants Buckley commits. Follow repo instructions first, then the user's latest instruction.

## Going deeper

- Public source: [odvcencio/buckley](https://github.com/odvcencio/buckley)
- `docs/CLI.md`: Read for command flags and surfaces.
- `docs/ORCHESTRATION.md`: Read for plan/execute workflows.
- `docs/TOOLS.md`: Read when configuring trust, approvals, or tool exposure.
