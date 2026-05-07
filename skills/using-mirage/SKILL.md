---
name: using-mirage
description: |
  Use this skill when encoding, decoding, inspecting, evaluating, or training
  Mirage .mrg image codecs, using TurboQuant latent streams, Manta-backed
  learned codecs, browser decoder assets, or Mirage Image v1 artifacts.
---

# Using Mirage

## When to reach for Mirage

Use Mirage when working with `.mrg` image artifacts, TurboQuant-native image compression, deterministic arithmetic-coded streams, learned Manta codec checkpoints, Kodak eval harnesses, or browser decoder assets.

Use it when the image pipeline needs to interoperate with Manta and TurboQuant instead of ordinary PNG/JPEG/WebP processing.

## What Mirage is

Mirage is the TurboQuant-native image codec surface. The standalone repo owns `.mrg` parsing, fixed v1 headers, CRC validation, arithmetic coding, q_norm streams, PNG/JPEG/PPM image I/O, WASM decoder assets, and CLI encode/decode/eval paths. Learned codec pieces are backed by Manta `.mll` modules and weights.

## Quick start

```bash
go run ./cmd/mirage encode -in input.png -out input.mrg -bits 4 -factorization categorical
go run ./cmd/mirage info -in input.mrg
go run ./cmd/mirage decode -in input.mrg -out decoded.png
go run ./cmd/mirage eval -source input.png -mrg input.mrg
```

Manta-backed paths:

```bash
go run ./cmd/mirage init-manta -out mirage_v1.mll
go run ./cmd/mirage check-manta -in mirage_v1.mll -entry train_step
go run ./cmd/mirage train-manta-smoke -in input.png -steps 24 -crop 16 -bits 2
```

## When NOT to use Mirage

Do not use Mirage when the task only needs a mature production web image codec such as PNG, JPEG, AVIF, or WebP. Do not treat current learned-codec baselines as final compression quality.

Do not move learned analysis/synthesis network semantics into Mirage unless the repo docs say that surface has moved from Manta.

## Going deeper

- Public source: [odvcencio/mirage](https://github.com/odvcencio/mirage)
- `docs/v1-implementation.md`: Read for current v1 codec behavior, training recipes, and measurement context.
- `cmd/mirage`: Read for exact CLI flags.
- `scripts/build_web.sh`: Use when producing browser decoder assets.
