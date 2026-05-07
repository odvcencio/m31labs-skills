---
name: using-gotreesitter
description: |
  Use this skill when implementing pure-Go tree-sitter parsing, language
  detection, queries, tags, highlights, incremental parsing, source rewrites,
  injection parsing, grammar blobs, or typed query code generation.
---

# Using gotreesitter

## When to reach for gotreesitter

Use gotreesitter when Go code needs tree-sitter semantics without CGo: cross-compilable parsing, syntax trees, S-expression queries, highlights, tags, language detection, incremental reparsing, injection parsing, or source rewrites.

Use it underneath code intelligence, editors, language tooling, and artifact generation where a C compiler dependency would be costly.

## What gotreesitter is

gotreesitter is a pure-Go tree-sitter runtime. It loads grammar blobs extracted from upstream `parser.c` files and implements parser, lexer, query engine, incremental edit handling, external scanners, tree cursors, tags, highlights, and injection parsing in Go.

## Quick start

```go
package main

import (
	"fmt"

	"github.com/odvcencio/gotreesitter"
	"github.com/odvcencio/gotreesitter/grammars"
)

func main() {
	src := []byte("package main\n\nfunc main() {}\n")
	lang := grammars.GoLanguage()
	parser := gotreesitter.NewParser(lang)
	tree, err := parser.Parse(src)
	if err != nil {
		panic(err)
	}
	fmt.Println(tree.RootNode())
}
```

Run a query:

```go
q, _ := gotreesitter.NewQuery(`(function_declaration name: (identifier) @fn)`, lang)
cursor := q.Exec(tree.RootNode(), lang, src)
for {
	match, ok := cursor.NextMatch()
	if !ok {
		break
	}
	for _, cap := range match.Captures {
		fmt.Println(cap.Node.Text(src))
	}
}
```

## When NOT to use gotreesitter

Do not use gotreesitter when literal text search is sufficient. Do not assume all grammars have tags or highlight queries; detect capability through `grammars.DetectLanguage` and the registry entry before requiring tags.

Do not hand-edit generated grammar blobs. Regenerate through the repo tooling.

## Going deeper

- Public source: [odvcencio/gotreesitter](https://github.com/odvcencio/gotreesitter)
- `cmd/tsquery`: Use for typed query wrappers from `.scm` files.
- `cmd/grammargen` and `cmd/ts2go`: Use when working on grammar generation.
- README sections "Query API", "Build tags and environment", and "Implementation notes" are the durable reference.
