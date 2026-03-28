# Awesome-KVCache

A curated list of papers, systems, benchmarks, and engineering resources for KV cache optimization in LLM inference.

This repository focuses on **inference-time KV cache research**: how to reduce KV memory, decide what to keep, reuse cached states across requests, and build serving systems around those ideas.

## Table of Contents

- [Scope](#scope)
- [Design Principles](#design-principles)
- [Taxonomy](#taxonomy)
- [Repository Structure](#repository-structure)
- [ArXiv Tracking](#arxiv-tracking)
- [Paper Inbox](#paper-inbox)
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
├── arxiv-tracker
│   ├── README.md
│   ├── note-template.md
│   └── notes
│       └── 2026
├── data
│   ├── arxiv-tracker.csv
│   ├── papers.csv
│   └── systems.csv
├── docs
│   ├── paper-inbox.md
│   ├── paper-template.md
│   ├── reading-roadmap.md
│   └── taxonomy.md
└── .github
    └── ISSUE_TEMPLATE
        └── add-paper.md
```

## ArXiv Tracking

To track very recent KV-cache-related papers without polluting the main curated list, this repo now uses a separate `arxiv-tracker/` module.

Recommended workflow:

1. Add a new paper to [`arxiv-tracker/README.md`](arxiv-tracker/README.md)
2. Add structured metadata to [`data/arxiv-tracker.csv`](data/arxiv-tracker.csv)
3. Create a short note under `arxiv-tracker/notes/<year>/`
4. Once the paper is clearly relevant and worth keeping, promote it into the main taxonomy in this README

This separation keeps the homepage stable while still giving you a place to track newly released arXiv papers aggressively.

## Paper Inbox

If you only want a quick place to drop paper titles before reading them carefully, use [`docs/paper-inbox.md`](docs/paper-inbox.md).

Recommended workflow:

1. Put the paper title in `docs/paper-inbox.md`
2. Read and summarize it
3. If it is truly KV-cache-centric, move it into the main taxonomy in this README

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
| 2024 | [MatryoshkaKV: Adaptive KV Compression via Trainable Orthogonal Projection](https://arxiv.org/abs/2410.14731) | ICLR 2025 | Compresses KV hidden dimensions with trainable orthogonal projection rather than deleting tokens. | decode, long-context | memory↓, quality retained | N/A |
| 2024 | [RazorAttention: Efficient KV Cache Compression Through Retrieval Heads](https://arxiv.org/abs/2407.15891) | ICLR 2025 | Preserves full remote KV only for retrieval heads and compresses the rest with a training-free strategy. | decode, long-context | memory↓, quality retained | N/A |
| 2024 | [SnapKV: LLM Knows What You are Looking for Before Generation](https://arxiv.org/abs/2404.14469) | arXiv | Uses an observation window to identify important KV positions per head before generation. | prefill, decode, long-context | memory↓, latency↓ | [repo](https://github.com/FasterDecoding/SnapKV) |
| 2024 | [MiniCache: KV Cache Compression in Depth Dimension for Large Language Models](https://arxiv.org/abs/2405.14366) | arXiv | Compresses KV across layers by exploiting cross-layer similarity. | decode, long-context | memory↓ | [repo](https://github.com/ydchen0806/MiniCache) |
| 2024 | [PyramidKV: Dynamic KV Cache Compression based on Pyramidal Information Funneling](https://openreview.net/forum?id=jZVNmDiU86) | ICLR 2025 | Allocates cache differently across layers based on pyramidal information flow. | decode, long-context | memory↓, quality retained | [repo](https://github.com/Zefan-Cai/PyramidKV) |
| 2024 | [Palu: KV-Cache Compression with Low-Rank Projection](https://arxiv.org/abs/2407.21118) | ICLR 2025 | Uses low-rank projection to compress KV hidden dimensions while preserving long-context behavior. | decode, long-context | memory↓, latency↓ | N/A |
| 2025 | [RocketKV: Accelerating Long-Context LLM Inference via Two-Stage KV Cache Compression](https://proceedings.mlr.press/v267/behnam25a.html) | ICML 2025 | Combines permanent coarse-grain eviction with fine-grain sparse attention in a two-stage compression pipeline. | decode, long-context | memory↓, latency↓, throughput↑ | N/A |
| 2025 | [Lexico: Extreme KV Cache Compression via Sparse Coding over Universal Dictionaries](https://proceedings.mlr.press/v267/kim25ae.html) | ICML 2025 | Compresses KV states with sparse coding over shared universal dictionaries instead of token dropping or low-bit scalar quantization. | decode, long-context | memory↓, quality retained | N/A |
| 2025 | [ClusterAttn: KV Cache Compression under Intrinsic Attention Clustering](https://aclanthology.org/2025.acl-long.703/) | ACL 2025 | Uses head-specific attention clustering to compress prompt KV more precisely than uniform sparse patterns. | prefill, decode, long-context | memory↓, latency↓, throughput↑ | N/A |
| 2025 | [FreqKV: Key-Value Compression in Frequency Domain for Context Window Extension](https://arxiv.org/abs/2505.00570) | ICLR 2026 | Iteratively compresses KV in the frequency domain to support robust long-context extension. | prefill, decode, long-context | context length↑, quality retained | [repo](https://github.com/LUMIA-Group/FreqKV) |
| 2025 | [KV Cache Transform Coding for Compact Storage in LLM Inference](https://arxiv.org/abs/2511.01815) | ICLR 2026 | Uses transform coding, adaptive quantization, and entropy coding for compact reusable KV storage. | serving, reusable-cache | memory↓, compression↑ | N/A |
| 2026 | [KVSlimmer: Theoretical Insights and Practical Optimizations for Asymmetric KV Merging](https://arxiv.org/abs/2603.00907) | arXiv | Gives a theory-backed, gradient-free closed-form method for asymmetric KV merging. | decode, long-context | memory↓, latency↓, quality↑ | N/A |

### 2. Quantization

Methods that reduce KV precision, often with asymmetric or mixed-precision designs.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2024 | [KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache](https://arxiv.org/abs/2402.02750) | ICML 2024 | Uses asymmetric 2-bit quantization with different strategies for keys and values. | decode, serving | memory↓, throughput↑ | [repo](https://github.com/jy-yuan/KIVI) |
| 2024 | [KVQuant: Towards 10 Million Context Length LLM Inference with KV Cache Quantization](https://arxiv.org/abs/2401.18079) | arXiv | Pushes sub-4-bit KV quantization for ultra-long context inference. | long-context, serving | memory↓, context length↑ | [repo](https://github.com/SqueezeAILab/KVQuant) |
| 2025 | [KVTuner: Sensitivity-Aware Layer-Wise Mixed-Precision KV Cache Quantization for Efficient and Nearly Lossless LLM Inference](https://arxiv.org/abs/2502.04420) | ICML 2025 | Searches hardware-friendly layer-wise mixed-precision KV bit-widths to improve throughput with little quality loss. | decode, serving | throughput↑, latency↓ | [repo](https://github.com/cmd2001/KVTuner) |
| 2025 | [CommVQ: Commutative Vector Quantization for KV Cache Compression](https://proceedings.mlr.press/v267/li25du.html) | ICML 2025 | Uses RoPE-compatible vector quantization to compress KV states with better positional robustness than scalar low-bit methods. | decode, long-context | memory↓, quality retained | N/A |
| 2025 | [MiniKV: Pushing the Limits of 2-Bit KV Cache via Compression and System Co-Design for Efficient Long Context Inference](https://aclanthology.org/2025.findings-acl.952/) | Findings of ACL 2025 | Combines 2-bit KV quantization, adaptive KV policies, and hardware-friendly kernels for practical long-context speedups. | decode, long-context, serving | memory↓, latency↓, throughput↑ | N/A |
| 2025 | [XQuant: Achieving Ultra-Low Bit KV Cache Quantization with Cross-Layer Compression](https://aclanthology.org/2025.emnlp-main.494/) | EMNLP 2025 | Pushes KV quantization below 1.4 bits with data-free calibration and cross-layer compression. | decode, long-context | memory↓, quality retained | [repo](https://github.com/brinenick511/XQuant) |

### 3. Eviction and Budget Allocation

Methods that explicitly manage a fixed cache budget and decide what to discard.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2024 | [On the Efficacy of Eviction Policy for Key-Value Constrained Generative Language Model Inference](https://arxiv.org/abs/2402.06262) | arXiv | Studies why prior eviction policies fail and proposes a more robust omission strategy. | decode, long-context | quality retained under budget | N/A |
| 2024 | [SqueezeAttention: 2D Management of KV-Cache in LLM Inference via Layer-wise Optimal Budget](https://arxiv.org/abs/2404.04793) | ICLR 2025 | Jointly allocates KV budget across layers and tokens instead of treating every layer equally. | decode, long-context | memory↓, throughput↑ | [repo](https://github.com/hetailang/SqueezeAttention) |
| 2025 | [Lookahead Q-Cache: Achieving More Consistent KV Cache Eviction via Pseudo Query](https://arxiv.org/abs/2505.20334) | EMNLP 2025 | Uses low-cost pseudo lookahead queries to make KV eviction more consistent with true decode-time needs. | decode, long-context | quality↑ under budget | N/A |
| 2025 | [CAKE: Cascading and Adaptive KV Cache Eviction with Layer Preferences](https://proceedings.iclr.cc/paper_files/paper/2025/hash/dfae940651f3e690a12e19c874edad7c-Abstract-Conference.html) | ICLR 2025 | Allocates cache across layers using adaptive layer-specific preferences. | decode, long-context | memory↓, latency↓ | [repo](https://github.com/antgroup/cakekv) |
| 2025 | [KeyDiff: Key Similarity-Based KV Cache Eviction for Long-Context LLM Inference in Resource-Constrained Environments](https://arxiv.org/abs/2504.15364) | NeurIPS 2025 | Uses key similarity instead of attention scores to preserve diverse and useful cache states under a strict budget. | decode, long-context | latency↓, quality retained | N/A |

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
| 2024 | [Marconi: Prefix Caching for the Era of Hybrid LLMs](https://proceedings.mlsys.org/paper_files/paper/2025/hash/7c180af017258d239bac6248d1eb26ac-Abstract-Conference.html) | MLSys 2025 | Extends prefix caching to hybrid attention-recurrent models with reuse-aware admission and eviction. | prefill, serving | token hit rate↑, TTFT↓ | N/A |
| 2025 | [KVCache Cache in the Wild: Characterizing and Optimizing KVCache Cache at a Large Cloud Provider](https://www.usenix.org/conference/atc25/presentation/wang-jiahao) | USENIX ATC 2025 | Characterizes real production KV-cache reuse patterns and derives a workload-aware eviction policy. | prefill, serving | hit rate↑, throughput↑, latency↓ | N/A |
| 2024 | [LMCache Documentation](https://docs.lmcache.ai/) | System | Provides a reusable KV layer for prefill-once, reuse-everywhere serving workflows. | prefill, serving | TTFT↓, GPU cycles↓ | [repo](https://github.com/LMCache/LMCache) |

### 6. Memory Management and Cache Layout

Serving-engine work on allocation, fragmentation, sharing, and layout abstractions.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2023 | [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/abs/2309.06180) | SOSP 2023 | Uses paged KV blocks and copy-on-write sharing to reduce fragmentation and duplication. | serving | throughput↑, memory waste↓ | [repo](https://github.com/vllm-project/vllm) |
| 2026 | [KEEP: A KV-Cache-Centric Memory Management System for Efficient Embodied Planning](https://arxiv.org/abs/2602.23592) | DAC 2026 | Manages dynamic embodied-planning memory directly at the KV-cache layer to reduce recomputation and loading imbalance. | prefill, serving | TTFT↓, speed↑ | [repo](https://github.com/PKU-SEC-Lab/KEEP_Embodied_Memory) |

### 7. Offloading and Disaggregated Serving

Work on moving KV between different devices or between prefill and decode nodes.

| Year | Paper | Venue | Core Idea | Setting | Gain | Code |
| --- | --- | --- | --- | --- | --- | --- |
| 2025 | [FlowKV: A Disaggregated Inference Framework with Low-Latency KV Cache Transfer and Load-Aware Scheduling](https://arxiv.org/abs/2504.03775) | arXiv | Optimizes KV transfer and scheduling in disaggregated prefill/decode serving. | serving, disaggregation | transfer latency↓, throughput↑ | N/A |
| 2024 | [NEO: Saving GPU Memory Crisis with CPU Offloading for Online LLM Inference](https://proceedings.mlsys.org/paper_files/paper/2025/hash/66a026c0d17040889b50f0dfa650e5e0-Abstract-Conference.html) | MLSys 2025 | Offloads part of attention compute and KV states to CPU with asymmetric GPU-CPU pipelining and load-aware scheduling. | decode, serving, cpu-offload | throughput↑, GPU memory pressure↓ | N/A |
| 2024 | [ShadowKV: KV Cache in Shadows for High-Throughput Long-Context LLM Inference](https://proceedings.mlr.press/v267/sun25b.html) | ICML 2025 | Keeps low-rank key shadows on GPU, offloads values, and reconstructs sparse KV on demand for higher long-context throughput. | decode, serving, cpu-offload | throughput↑, memory↓ | N/A |
| 2025 | [SpeCache: Speculative Key-Value Caching for Efficient Generation of LLMs](https://arxiv.org/abs/2503.16163) | ICML 2025 | Offloads the full KV cache to CPU memory and speculatively prefetches likely-needed states back to VRAM. | decode, serving | VRAM↓, quality retained | N/A |
| 2025 | [TailorKV: A Hybrid Framework for Long-Context Inference via Tailored KV Cache Optimization](https://aclanthology.org/2025.findings-acl.1043/) | Findings of ACL 2025 | Combines dominant-token loading and quantization at different layers to balance PCIe cost and compression error. | decode, long-context, cpu-offload | latency↓, memory↓ | N/A |
| 2025 | [HGCA: Hybrid GPU-CPU Attention for Long Context LLM Inference](https://arxiv.org/abs/2507.03153) | arXiv | Runs dense recent attention on GPU and sparse salient attention on CPU, then fuses the results with a hybrid attention path. | decode, long-context, cpu-offload | memory↓, PCIe overhead↓ | N/A |
| 2025 | [Mooncake: Trading More Storage for Less Computation — A KVCache-centric Architecture for Serving LLM Chatbot](https://www.usenix.org/conference/fast25/presentation/qin) | FAST 2025 | Builds a KV-cache-centric serving architecture around disaggregated cache storage and SLO-aware scheduling. | serving, disaggregation, reusable-cache | TTFT↓, throughput↑ | N/A |
| 2026 | [IceCache: Memory-Efficient KV-cache Management for Long-Sequence LLMs](https://openreview.net/forum?id=yHxSKM9kdr) | ICLR 2026 | Combines semantic token clustering, paging, and CPU-GPU transfer-aware KV management for long sequences. | decode, long-context, cpu-offload | memory↓, quality retained | N/A |
| 2026 | [DualPath: Breaking the Storage Bandwidth Bottleneck in Agentic LLM Inference](https://arxiv.org/abs/2602.21548) | arXiv | Adds a storage-to-decode KV loading path and global scheduling for disaggregated agentic inference. | serving, disaggregation | throughput↑, SLO-safe online serving | N/A |
| 2026 | [Bidaw: Enhancing Key-Value Caching for Interactive LLM Serving via Bidirectional Computation-Storage Awareness](https://www.usenix.org/conference/fast26/presentation/hu-shipeng) | FAST 2026 | Coordinates host-memory and SSD KV tiers with compute-aware scheduling for interactive multi-turn serving. | serving, disaggregation, reusable-cache | latency↓, cache efficiency↑ | N/A |
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
- Use [`arxiv-tracker/note-template.md`](arxiv-tracker/note-template.md) for per-paper notes
- Submit an issue or PR using the template in [`.github/ISSUE_TEMPLATE/add-paper.md`](.github/ISSUE_TEMPLATE/add-paper.md)

## Related Awesome Lists

- [Awesome-Speculative-Decoding](https://github.com/Jiawei888/Awesome-Speculative-Decoding)
- [Awesome-LLM-Inference](https://github.com/xlite-dev/Awesome-LLM-Inference)
