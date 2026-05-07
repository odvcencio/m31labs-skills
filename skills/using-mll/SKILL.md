---
name: using-mll
description: |
  Use this skill when reading, writing, hashing, signing, validating, or
  inspecting MLL binary model artifacts, sealed inference artifacts,
  weights-only bundles, checkpoints, tensor sections, or deterministic content
  hashes.
---

# Using MLL

## When to reach for MLL

Use MLL when an ML artifact needs deterministic binary packaging: tensor declarations, raw tensor bytes, entry points, memory hints, optimizer state, signatures, sealed content hashes, weights-only bundles, or checkpoint saves.

Use MLL as the artifact container underneath Manta or other model/runtime systems, not as a model execution runtime by itself.

## What MLL is

MLL is a sectioned binary interchange format and Go module for ML artifacts. Files have a fixed header, a section directory, BLAKE3 section digests, canonical ordering for sealed artifacts, reproducible content hashes, profile validation, and optional Ed25519 signatures.

## Quick start

```bash
go get github.com/odvcencio/mll
go run ./cmd/mll inspect model.mllb
```

```go
r, err := mll.ReadFile("model.mllb", mll.WithDigestVerification())
if err != nil {
	return err
}
if err := r.Validate(); err != nil {
	return err
}
body, ok := r.Section(mll.TagTNSR)
if ok {
	tensors, err := mll.ReadTnsrSection(body)
	if err != nil {
		return err
	}
	_ = tensors
}
```

Build a simple sealed artifact:

```go
artifact := mll.NewSealedArtifact("tiny_embed")
artifact.AddDim("D", 384)
weights := make([]byte, 384*4)
if err := artifact.AddTensor("token_embedding", mll.DTypeF32, []uint64{384}, weights); err != nil {
	return err
}
data, contentHash, err := artifact.Marshal()
_, _, _ = data, contentHash, err
```

## When NOT to use MLL

Do not use MLL as a general archive format. Do not skip digest verification when loading artifacts across a trust boundary. Do not mutate sealed artifacts in place; write a new artifact and compare content hashes.

Do not put runtime semantics in MLL that belong in Manta or another executor.

## Going deeper

- Public source: [odvcencio/mll](https://github.com/odvcencio/mll)
- README "Profiles": Read before choosing sealed, checkpoint, or weights-only.
- README "Test vectors": Read when validating compatibility.
- `cmd/mll inspect`: Use for header, directory, digest, and validation checks.
