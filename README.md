# Awesome-KVCache

A curated list of papers, systems, benchmarks, and engineering resources for KV cache optimization in LLM inference.

This repository focuses on **inference-time KV cache research**: how to reduce KV memory, decide what to keep, reuse cached states across requests, and build serving systems around those ideas.

## Table of Contents

- [Scope](#scope)
- [Design Principles](#design-principles)
- [Taxonomy](#taxonomy)
- [Repository Structure](#repository-structure)
- [Paper List by Category](#paper-list-by-category)
  - [1. Compression and Sparsification](#1-compression-and-sparsification)
  - [2. Quantization](#2-quantization)
  - [3. Eviction and Budget Allocation](#3-eviction-and-budget-allocation)
  - [4. Streaming and Fixed-Memory Decoding](#4-streaming-and-fixed-memory-decoding)
  - [5. Prefix Reuse and Prompt Cache](#5-prefix-reuse-and-prompt-cache)
  - [6. Memory Management and Cache Layout](#6-memory-management-and-cache-layout)
  - [7. Offloading and Disaggregated Serving](#7-offloading-and-disaggregated-serving)
  - [8. System and Kernel Co-design](#8-system-and-kernel-co-design)
- [Systems, Libraries, and Benchmarks](#systems-libraries-and-benchmarks)
- [Reading Roadmap](#reading-roadmap)
- [How to Contribute](#how-to-contribute)
- [Related Awesome Lists](#related-awesome-lists)

## Scope

We include work whose main contribution is clearly related to one or more of the following:

- Reducing KV cache memory footprint
- Quantizing or compressing KV states
- Evicting, selecting, or allocating KV budget
- Reusing KV across requests, sessions, or branches
- Managing KV placement across GPU, CPU, storage, or network
- Designing cache-aware serving systems, schedulers, or kernels

We do **not** use "KV cache" as a catch-all label for every long-context paper. If a paper mainly proposes a new attention architecture, retrieval method, or model training recipe without a KV-cache-centric contribution, it should usually stay out of the main list.

## Design Principles

- **Organize by problem** first, not by paper venue.
- **Keep module boundaries stable** so later additions do not require restructuring the repo.
- **Prefer one primary category** per paper. Cross-links are fine; duplication should stay minimal.
- **Make README readable** and move longer explanations to [`docs/taxonomy.md`](docs/taxonomy.md).

## Taxonomy

The repo uses eight top-level modules.

| Module | Focus | Typical Questions |
| --- | --- | --- |
| Compression and Sparsification | Reduce the number or representation of cached KV states | Which tokens, heads, or layers matter enough to keep? |
| Quantization | Lower KV precision with minimal quality loss | Can we push KV to 4-bit, 2-bit, or mixed precision? |
| Eviction and Budget Allocation | Decide what to retain under a fixed KV budget | If memory is capped, what should be dropped first? |
| Streaming and Fixed-Memory Decoding | Maintain bounded memory for very long or infinite streams | How do we decode with constant or near-constant KV growth? |
| Prefix Reuse and Prompt Cache | Reuse prefill results across requests or sessions | How do we avoid recomputing shared context? |
| Memory Management and Cache Layout | Allocate, share, and place KV efficiently in serving engines | How should KV blocks be laid out and shared safely? |
| Offloading and Disaggregated Serving | Move KV across GPU/CPU/storage/network tiers | When and where should KV be transferred or persisted? |
| System and Kernel Co-design | Co-design kernels, layouts, and runtimes around KV access patterns | How do quantization/layout ideas translate into actual throughput? |

Detailed boundary notes and paper-placement rules live in [`docs/taxonomy.md`](docs/taxonomy.md).

## Repository Structure

```text
.
├── README.md
├── CONTRIBUTING.md
├── docs
│   ├── paper-template.md
│   ├── reading-roadmap.md
│   └── taxonomy.md
├── data
│   ├── papers.csv
│   └── systems.csv
└── .github
    └── ISSUE_TEMPLATE
        └── add-paper.md
```

## Paper List by Category

Table schema:

- `Year`: publication year
- `Paper`: title with paper link
- `Venue`: conference, journal, or `arXiv`
- `Core Idea`: one-sentence contribution
- `Setting`: `prefill`, `decode`, `long-context`, `serving`, or a combination
- `Gain`: concise summary such as `memory↓`, `latency↓`, `throughput↑`
- `Code`: official repo if available

The tables below are **seed entries** to anchor the taxonomy, not an exhaustive list yet.

### 1. Compression and Sparsification

Methods that directly compress KV states or keep only a subset of tokens, heads, or layers.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2023 | [H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models](https://proceedings.neurips.cc/paper_files/paper/2023/hash/6ceefa7b15572587b78ecfcebb2827f8-Abstract-Conference.html) | NeurIPS | Retains heavy-hitter tokens that contribute most to attention. | decode, long-context | memory↓, throughput↑ | [repo](https://github.com/FMInference/H2O) |
| 2024 | [SnapKV: LLM Knows What You are Looking for Before Generation](https://arxiv.org/abs/2404.14469) | arXiv | Uses an observation window to identify important KV positions per head before generation. | prefill, decode, long-context | memory↓, latency↓ | [repo](https://github.com/FasterDecoding/SnapKV) |
| 2024 | [MiniCache: KV Cache Compression in Depth Dimension for Large Language Models](https://arxiv.org/abs/2405.14366) | arXiv | Compresses KV across layers by exploiting cross-layer similarity. | decode, long-context | memory↓ | [repo](https://github.com/ydchen0806/MiniCache) |
| 2024 | [PyramidKV: Dynamic KV Cache Compression based on Pyramidal Information Funneling](https://openreview.net/forum?id=jZVNmDiU86) | ICLR 2025 | Allocates cache differently across layers based on pyramidal information flow. | decode, long-context | memory↓, quality retained | [repo](https://github.com/Zefan-Cai/PyramidKV) |

### 2. Quantization

Methods that reduce KV precision, often with asymmetric or mixed-precision designs.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2024 | [KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache](https://arxiv.org/abs/2402.02750) | ICML 2024 | Uses asymmetric 2-bit quantization with different strategies for keys and values. | decode, serving | memory↓, throughput↑ | [repo](https://github.com/jy-yuan/KIVI) |
| 2024 | [KVQuant: Towards 10 Million Context Length LLM Inference with KV Cache Quantization](https://arxiv.org/abs/2401.18079) | arXiv | Pushes sub-4-bit KV quantization for ultra-long context inference. | long-context, serving | memory↓, context length↑ | [repo](https://github.com/SqueezeAILab/KVQuant) |

### 3. Eviction and Budget Allocation

Methods that explicitly manage a fixed cache budget and decide what to discard.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2024 | [On the Efficacy of Eviction Policy for Key-Value Constrained Generative Language Model Inference](https://arxiv.org/abs/2402.06262) | arXiv | Studies why prior eviction policies fail and proposes a more robust omission strategy. | decode, long-context | quality retained under budget | N/A |
| 2025 | [CAKE: Cascading and Adaptive KV Cache Eviction with Layer Preferences](https://proceedings.iclr.cc/paper_files/paper/2025/hash/dfae940651f3e690a12e19c874edad7c-Abstract-Conference.html) | ICLR 2025 | Allocates cache across layers using adaptive layer-specific preferences. | decode, long-context | memory↓, latency↓ | [repo](https://github.com/antgroup/cakekv) |

### 4. Streaming and Fixed-Memory Decoding

Methods that keep decoding stable when context keeps growing.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2023 | [Efficient Streaming Language Models with Attention Sinks](https://arxiv.org/abs/2309.17453) | arXiv | Shows that keeping sink tokens plus a local window enables stable streaming inference. | streaming, decode | memory bounded, speed↑ | [repo](https://github.com/mit-han-lab/streaming-llm) |

### 5. Prefix Reuse and Prompt Cache

Methods and systems that reuse shared prompt computation across requests.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2024 | [Preble: Efficient Distributed Prompt Scheduling for LLM Serving](https://arxiv.org/abs/2407.00023) | arXiv | Schedules requests to maximize prefix reuse while balancing load in distributed serving. | prefill, serving | TTFT↓, latency↓ | N/A |
| 2024 | [LMCache Documentation](https://docs.lmcache.ai/) | System | Provides a reusable KV layer for prefill-once, reuse-everywhere serving workflows. | prefill, serving | TTFT↓, GPU cycles↓ | [repo](https://github.com/LMCache/LMCache) |

### 6. Memory Management and Cache Layout

Serving-engine work on allocation, fragmentation, sharing, and layout abstractions.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2023 | [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/abs/2309.06180) | SOSP 2023 | Uses paged KV blocks and copy-on-write sharing to reduce fragmentation and duplication. | serving | throughput↑, memory waste↓ | [repo](https://github.com/vllm-project/vllm) |

### 7. Offloading and Disaggregated Serving

Work on moving KV between different devices or between prefill and decode nodes.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2025 | [FlowKV: A Disaggregated Inference Framework with Low-Latency KV Cache Transfer and Load-Aware Scheduling](https://arxiv.org/abs/2504.03775) | arXiv | Optimizes KV transfer and scheduling in disaggregated prefill/decode serving. | serving, disaggregation | transfer latency↓, throughput↑ | N/A |
| 2025 | [Disaggregated Serving](https://docs.nvidia.com/dynamo/v-0-7-0/design-docs/disaggregated-serving) | System | NVIDIA Dynamo design doc on prefill/decode separation and KV transfer paths. | serving, disaggregation | system design reference | N/A |

### 8. System and Kernel Co-design

Work where KV-aware kernels or systems are the main driver of end-to-end speedups.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2024 | [QServe: W4A8KV4 Quantization and System Co-design for Efficient LLM Serving](https://arxiv.org/abs/2405.04532) | MLSys 2025 | Fuses low-bit weight/activation/KV design with a serving stack. | serving | throughput↑, efficiency↑ | [repo](https://github.com/mit-han-lab/omniserve) |

## Systems, Libraries, and Benchmarks

Initial non-paper artifacts worth tracking in separate lists:

- **Serving systems**: vLLM, SGLang, TensorRT-LLM, LMCache, Dynamo
- **Benchmark suites**: LongBench, NeedleBench, TTFT-centric serving benchmarks
- **Metrics**:
  - Memory footprint per token
  - Effective max context under fixed hardware
  - TTFT
  - Decode latency
  - Throughput under batching
  - Quality loss under fixed cache budget

These should eventually be tracked in [`data/systems.csv`](data/systems.csv).

## Reading Roadmap

Three practical entry points:

1. **If you care about algorithmic KV reduction**
   Start with H2O, SnapKV, MiniCache, PyramidKV.
2. **If you care about deployment and serving**
   Start with PagedAttention/vLLM, Preble, LMCache, FlowKV.
3. **If you care about low-bit inference**
   Start with KIVI, KVQuant, QServe.

See [`docs/reading-roadmap.md`](docs/reading-roadmap.md) for a more structured reading order.

## How to Contribute

If you want to add papers, systems, or benchmarks:

- Read [`CONTRIBUTING.md`](CONTRIBUTING.md)
- Use [`docs/paper-template.md`](docs/paper-template.md) for new entries
- Submit an issue or PR using the template in [`.github/ISSUE_TEMPLATE/add-paper.md`](.github/ISSUE_TEMPLATE/add-paper.md)

## Related Awesome Lists

- [Awesome-Speculative-Decoding](https://github.com/Jiawei888/Awesome-Speculative-Decoding)
- [Awesome-LLM-Inference](https://github.com/xlite-dev/Awesome-LLM-Inference)
