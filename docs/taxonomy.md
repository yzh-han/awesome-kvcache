# Taxonomy for Awesome-KVCache

This document explains how papers should be placed into modules.

## Placement Rule

Assign each paper to the category that best reflects its **primary contribution**.

- If a paper mainly changes which KV states are stored, it usually belongs to `Compression and Sparsification`.
- If it mainly lowers bit-width, it belongs to `Quantization`.
- If it mainly chooses what to drop under a budget, it belongs to `Eviction and Budget Allocation`.
- If it mainly targets unbounded streams or fixed-memory generation, it belongs to `Streaming and Fixed-Memory Decoding`.
- If it mainly reuses prompt or session context across requests, it belongs to `Prefix Reuse and Prompt Cache`.
- If it mainly redesigns KV allocation, sharing, or fragmentation handling in a runtime, it belongs to `Memory Management and Cache Layout`.
- If it mainly moves KV across devices, nodes, or storage tiers, it belongs to `Offloading and Disaggregated Serving`.
- If the core result depends on a kernel/runtime co-design around KV access patterns, it belongs to `System and Kernel Co-design`.

## Module Boundaries

### 1. Compression and Sparsification

Include:

- Token pruning
- Head-aware selection
- Layer-wise compression
- State merging or clustering
- Depth-dimension or structure-aware KV reduction

Exclude:

- Pure precision reduction without structural compression
- Papers whose main point is cache scheduling rather than what to keep

### 2. Quantization

Include:

- INT8, INT4, INT2, mixed-precision KV
- Asymmetric key/value quantization
- Per-token, per-channel, per-head quantization
- Quantization-aware kernels if the paper's novelty starts from KV quantization

Exclude:

- General LLM quantization papers with no KV-specific contribution

### 3. Eviction and Budget Allocation

Include:

- Budgeted eviction
- Importance scoring
- Retention policies
- Adaptive allocation across layers or heads

Exclude:

- Prefix reuse systems
- Memory allocators without an eviction contribution

### 4. Streaming and Fixed-Memory Decoding

Include:

- Sliding-window decoding
- Attention sinks
- Rolling cache
- Constant-memory or near-constant-memory generation

Exclude:

- Generic long-context methods not centered on runtime KV handling

### 5. Prefix Reuse and Prompt Cache

Include:

- Prefix caching
- Prompt deduplication
- Session reuse
- Shared prefill systems
- Cache reuse across users, workflows, or branches

Exclude:

- Pure paging or allocation papers with no reuse objective

### 6. Memory Management and Cache Layout

Include:

- Paged KV blocks
- Fragmentation reduction
- Copy-on-write sharing
- Cache-aware allocators
- Block tables and placement abstractions

Exclude:

- Cross-device transfer papers where movement is the main topic

### 7. Offloading and Disaggregated Serving

Include:

- GPU-CPU KV offload
- Disk or remote-memory backed KV
- Prefill/decode disaggregation
- KV transfer protocols and schedulers

Exclude:

- Single-device cache layout work

### 8. System and Kernel Co-design

Include:

- Fused dequant + attention
- KV-aware kernels
- Runtime and layout co-design
- System papers where throughput gains depend on KV access optimization

Exclude:

- Pure algorithm papers that do not materially change the runtime stack

## Secondary Tags

To avoid duplication across sections, use secondary tags in structured metadata instead of duplicating rows in README.

Recommended tags:

- `prefill`
- `decode`
- `long-context`
- `streaming`
- `serving`
- `branching`
- `multi-tenant`
- `disaggregation`
- `quantized`

## Suggested Future Extensions

As the list grows, it may help to add optional views:

- By scenario: single-request long context, batch serving, multi-turn chat, multi-tenant serving
- By artifact: paper, code, benchmark, system, blog, design doc
- By maturity: idea, benchmarked prototype, production-ready system

