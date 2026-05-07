---
name: using-gosx-collaboration
description: |
  Use this skill when building GoSX realtime collaboration, WebSocket hubs,
  presence, fanout, shared state, CRDT documents, CRDT sync, vector-backed
  workspaces, multi-agent collaboration, semantic shared findings, or
  conflict-free document editing.
---

# Using GoSX Collaboration

## When to reach for GoSX collaboration

Use GoSX collaboration primitives when multiple clients, agents, or browser tabs need to share state: presence, broadcast events, shared document edits, semantic findings, workspace search, and conflict-free offline/partition recovery.

Use a plain action or API route for one-shot mutations. Add hubs and CRDTs only when realtime shared state or convergence matters.

## What GoSX collaboration is

GoSX collaboration has three main layers:

- `hub`: WebSocket presence, client lifecycle, message framing, typed dispatch, broadcast, and per-connection state.
- `crdt`: conflict-free replicated document model with save/load and sync protocol.
- `workspace`: distributed semantic collaboration space built on CRDT, hub, and vecdb for shared findings and similarity search.

## Quick start

Create a hub and broadcast updates:

```go
package app

import "github.com/odvcencio/gosx/hub"

func WorkspaceHub() *hub.Hub {
    h := hub.New("workspace")
    h.On("update", func(ctx *hub.Context) {
        h.Broadcast("state", map[string]string{"status": "updated"})
    })
    return h
}
```

Create and persist a CRDT document:

```go
doc := crdt.NewDoc()
doc.Apply(change)
snapshot, err := doc.Save()
if err != nil {
    return err
}
_ = snapshot
```

Use workspace when the shared state also needs semantic retrieval across participants.

## When NOT to use GoSX collaboration

Do not use hubs for data that must survive process restarts unless you also persist state.

Do not use CRDTs for simple server-owned forms where last-write-wins is acceptable and easier to reason about.

Do not use workspace or vector search when the collaboration problem is only presence or chat fanout.

Do not put authorization only in hub messages. Check access before connecting and before applying privileged operations.

## Going deeper

- `hub.New(name)`: realtime channel for clients.
- `h.On(event, handler)`: typed message handling.
- `h.Broadcast(event, payload)`: fanout state.
- `crdt.NewDoc`, `Apply`, `Save`: conflict-free document state.
- `crdtsync.NewState`: sync sessions for exchanging document deltas.
- `workspace.New`: semantic multi-client workspace built from CRDT, hub, and vecdb.
- Public source: [odvcencio/gosx](https://github.com/odvcencio/gosx)
