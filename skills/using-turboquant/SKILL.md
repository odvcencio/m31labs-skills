---
name: using-turboquant
description: |
  Use this skill when compressing float vectors, choosing MSE or
  inner-product-preserving quantization, scoring quantized corpora, using
  prepared-query top-k, quantized KV caches, WebGPU/CUDA scoring, or portable
  TurboQuant serialization.
---

# Using TurboQuant

## When to reach for TurboQuant

Use TurboQuant when vectors need compact storage with useful reconstruction or direct inner-product scoring: retrieval indexes, KV-cache compression, local-model memory, Manta/CorkScrewDB integration, browser WebGPU scoring, or CUDA prepared-query top-k.

Choose `Quantizer` for reconstruction-oriented MSE quantization. Choose `IPQuantizer` for repeated similarity search against a quantized corpus.

## What TurboQuant is

TurboQuant is a Go implementation of MSE-optimal and inner-product-preserving vector quantization. It compresses float32 vectors to 1-8 bits per dimension, supports direct inner-product estimates without full dequantization, and can run without CGo.

## Quick start

```go
import "github.com/odvcencio/turboquant"

q := turboquant.New(384, 2)
packed, norm := q.Quantize(vec)
recovered := q.Dequantize(packed)
for i := range recovered {
	recovered[i] *= norm
}
dot := q.InnerProduct(packed, norm, queryVec)
_ = dot
```

For repeated similarity search:

```go
ipq := turboquant.NewIP(384, 3)
qx := ipq.Quantize(vec)
pq := ipq.PrepareQuery(queryVec)
score := ipq.InnerProductPrepared(qx, pq)
_ = score
```

## When NOT to use TurboQuant

Do not use TurboQuant when exact vector reconstruction or exact nearest-neighbor ranking is required. Do not compare bit widths without holding dimension, method, seed, and scoring mode constant.

Do not assume GPU support is available on every platform. WebGPU is `js/wasm`; CUDA is experimental behind the `cuda` build tag on supported Linux hosts.

## Going deeper

- Public source: [odvcencio/turboquant](https://github.com/odvcencio/turboquant)
- README "IP-optimal quantization": Read for retrieval search.
- README "KV cache": Read for transformer memory work.
- README "Portable quantizer specs and index buffers": Read for storage or non-Go interop.
