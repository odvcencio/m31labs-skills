---
name: using-graft
description: |
  Use this skill when the user is working with Graft structural version control,
  entity-level diffs or merges, coordination sessions, work claims, structural
  blame, Graft modules, Orchard remotes, or Graft MCP/coordd workflows.
---

# Using Graft

## When to reach for Graft

Use Graft when line-level Git is too coarse: independent edits in the same file, import-block merges, structural review, entity blame, agent coordination, work claims, shared plans, or governed command execution tied to version control.

Use `graft coord` before committing in repos that require coordination checks.

## What Graft is

Graft is structural version control powered by gotreesitter. It decomposes source files into entities such as imports, declarations, methods, classes, and interstitial text, then diffs and merges by entity identity instead of line ranges. It also provides a coordination runtime for humans and agents.

## Quick start

```bash
go install github.com/odvcencio/graft/cmd/graft@latest
graft init myproject
cd myproject
graft add main.go
graft commit -m "initial commit"
```

Coordination workflow:

```bash
graft workon --as cedar
graft coord check
graft coord
graft workon --done --as cedar
```

Structural inspection:

```bash
graft diff --entity
graft diff --review
graft blame --entity path/to/file.go::decl:function_declaration::Name
```

## When NOT to use Graft

Do not replace a repo's normal Git workflow unless the repo or user asks for Graft. Do not ignore coordination conflicts that mention files or entities you are about to edit. Do not treat stale reclaim suggestions as required code changes; they are coordination metadata unless they block your work.

Do not use structural merge as proof of semantic correctness. Run the relevant tests after merges.

## Going deeper

- Public source: [odvcencio/graft](https://github.com/odvcencio/graft)
- `graft diff --help`: Read for entity and review modes.
- `graft coord --help`: Read for shared plans, notes, tasks, and checks.
- `graft mcp --help`: Read when exposing Graft to an AI host.
