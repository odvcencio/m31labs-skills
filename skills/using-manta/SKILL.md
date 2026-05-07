---
name: using-manta
description: |
  Use this skill when working with Manta .manta source, GPU inference kernels,
  embedding or reranking pipelines, retrieval scoring, decode-time inference,
  quantized tensors, MLL artifact export, or Manta training/evaluation flows.
---

# Using Manta

## When to reach for Manta

Use Manta when model-facing compute should be authored once and deployed through a portable artifact: embedding, reranking, retrieval-time scoring, decode-time inference, KV-cache operations, quantized tensor paths, or CorkScrewDB/Mirage integration.

Reach for Manta when `.mll` artifacts, backend-neutral schedules, or TurboQuant-native layouts matter more than writing a backend-specific CUDA or Metal program.

## What Manta is

Manta is an inference-first GPU language and runtime stack. It compiles `.manta` source into backend-neutral `.mll` execution plans. The compiler lowers HIR to MIR to LIR, then packages kernels, entrypoints, buffers, schedules, and metadata for CUDA, Metal, Vulkan, DirectML, WebGPU, or host fallback.

## Quick start

```bash
go install github.com/odvcencio/manta/cmd/manta@latest
```

```manta
param token_embedding: f16[V, D] @weight("weights/token_embedding")
param projection: f16[D, E] @weight("weights/projection")

kernel l2_normalize(x: f16[T, E]) -> f16[T, E] {
    return normalize(x)
}

pipeline embed(tokens: i32[T]) -> f16[T, E] {
    let hidden = gather(token_embedding, tokens)
    let projected = @matmul(hidden, projection)
    return l2_normalize(projected)
}
```

```bash
manta compile embed.manta embed.mll
manta inspect embed.mll
manta run embed.mll embed
```

Built-in demos:

```bash
manta demo tiny_embed
manta demo tiny_decode
manta demo tiny_score
manta demo tiny_rerank
```

## When NOT to use Manta

Do not use Manta for general-purpose GPU kernels that have no model or artifact boundary. Do not promise device execution on every backend; some backend surfaces may use host fallback until promoted kernels exist.

Do not treat training scoreboards as product guarantees. Use the checked-in gates, metrics files, and docs for the exact candidate being discussed.

## Going deeper

- Public source: [odvcencio/manta](https://github.com/odvcencio/manta)
- `docs/production-embedding.md`: Read for production embedding candidate flow.
- `docs/benchmarks.md`: Read for benchmark and scoreboard interpretation.
- `docs/local-long-context-embedder-wedge.md`: Read for the local long-context embedding target.
