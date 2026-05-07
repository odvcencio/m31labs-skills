---
name: using-corkscrewdb
description: |
  Use this skill when building with CorkScrewDB collections, text or vector
  search, version history, HLC point-in-time views, TurboQuant indexes,
  metadata filters, gRPC remote mode, federation, shards, or replication.
---

# Using CorkScrewDB

## When to reach for CorkScrewDB

Use CorkScrewDB when a Go application needs embedded or distributed vector search with version history: text-in and vector-in collection APIs, metadata filters, point-in-time reads, HLC history, quantized indexes, shard routing, gRPC access, or replication.

It is a strong fit for agent memory, retrieval stores, local-first vector state, and Manta/TurboQuant-backed retrieval systems.

## What CorkScrewDB is

CorkScrewDB is a pure-Go versioned vector database. It stores collection entries with append-only WAL persistence, snapshots, embedding-space enforcement, TurboQuant-backed flat and HNSW search, HLC versioning, and optional gRPC federation.

## Quick start

```go
package main

import (
	"fmt"
	"log"

	"github.com/odvcencio/corkscrewdb"
)

func main() {
	db, err := corkscrewdb.Open("./example.csdb")
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	coll := db.Collection("documents", corkscrewdb.WithBitWidth(2))
	err = coll.Put("doc-1", corkscrewdb.Entry{
		Text:     "the auth module uses WebAuthn passkeys",
		Metadata: map[string]string{"source": "review"},
	})
	if err != nil {
		log.Fatal(err)
	}

	results, err := coll.Search("passkeys", 5, corkscrewdb.Filter("source", "review"))
	if err != nil {
		log.Fatal(err)
	}
	for _, result := range results {
		fmt.Println(result.ID, result.Score, result.Text)
	}
}
```

Remote access uses the same collection API:

```go
db, err := corkscrewdb.Connect("127.0.0.1:4040", corkscrewdb.WithToken("agent-token-xxx"))
```

## When NOT to use CorkScrewDB

Do not use CorkScrewDB as a relational database. Do not reopen a persisted database with a different embedding provider or dimensionality; embedding-space config is enforced to protect search coherence.

Do not assume quantized search is exact nearest-neighbor search. It is optimized for compact retrieval and should be evaluated against the quality target.

## Going deeper

- Public source: [odvcencio/corkscrewdb](https://github.com/odvcencio/corkscrewdb)
- README "Remote Mode": Read before exposing or connecting over gRPC.
- README "Embedded Federation" and "Rebalancing": Read before shard or replication work.
- `cmd/memory`: Read when building agent-memory HTTP surfaces.
