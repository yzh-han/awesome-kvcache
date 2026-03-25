# ArXiv Tracking

This module is a fast-moving queue for **new or recent arXiv papers** related to KV cache.

Use this folder for papers that are:

- newly released
- still being triaged
- worth taking notes on before deciding whether they belong in the main awesome list

Once a paper is clearly relevant and important, it can be promoted to the main taxonomy in [`../README.md`](../README.md).

## Status

Use one of these tracking states:

- `watching`: paper noticed, not read carefully yet
- `noted`: short note has been written, still under review
- `promoted`: moved into the main curated list
- `rejected`: not central enough to KV cache for this repo

## File Naming

Recommended note filename:

```text
arxiv-tracker/notes/<year>/<yyyy-mm-dd>-<arxiv-id>-<short-slug>.md
```

Example:

```text
arxiv-tracker/notes/2026/2026-03-25-2603.12345-layerwise-kv-routing.md
```

## Accepted Batch: Jan-Feb 2026

Interpretation for this batch:

- Window: `2026-01-01` to `2026-02-28`
- Criterion: the paper has an arXiv version and an explicit acceptance signal visible from arXiv comments, OpenReview, or an official proceedings page during this window
- This is **not** restricted to papers first posted on arXiv in Jan-Feb 2026

| Added | Paper | Acceptance Signal | Status | Note | Outcome |
| --- | --- | --- | --- | --- | --- |
| 2026-03-25 | [KEEP: A KV-Cache-Centric Memory Management System for Efficient Embodied Planning](https://arxiv.org/abs/2602.23592) | `DAC 2026` in arXiv comments | promoted | [note](notes/2026/2026-03-25-2602.23592-keep.md) | accepted batch, main taxonomy |
| 2026-03-25 | [KV Cache Transform Coding for Compact Storage in LLM Inference](https://arxiv.org/abs/2511.01815) | [ICLR 2026 Poster](https://openreview.net/forum?id=aNVKROYpLB) | promoted | [note](notes/2026/2026-03-25-2511.01815-kvtc.md) | accepted batch, main taxonomy |
| 2026-03-25 | [FreqKV: Key-Value Compression in Frequency Domain for Context Window Extension](https://arxiv.org/abs/2505.00570) | [ICLR 2026 Poster](https://openreview.net/forum?id=wFSOtyvQ9d) | promoted | [note](notes/2026/2026-03-25-2505.00570-freqkv.md) | accepted batch, main taxonomy |

## Accepted Batch: 2025

Interpretation for this batch:

- Window: `2025-01-01` to `2025-12-31`
- Criterion: the paper has an arXiv version and an explicit acceptance signal visible from OpenReview, official proceedings, or ACL Anthology during 2025
- This is a curated set of the most directly KV-cache-centric accepted papers, not an exhaustive venue dump

| Added | Paper | Acceptance Signal | Status | Note | Outcome |
| --- | --- | --- | --- | --- | --- |
| 2026-03-25 | [MatryoshkaKV: Adaptive KV Compression via Trainable Orthogonal Projection](https://arxiv.org/abs/2410.14731) | [ICLR 2025 Poster](https://openreview.net/forum?id=BQwsRy1h3U) | promoted | [note](notes/2026/2026-03-25-2410.14731-matryoshkakv.md) | accepted batch, main taxonomy |
| 2026-03-25 | [SqueezeAttention: 2D Management of KV-Cache in LLM Inference via Layer-wise Optimal Budget](https://arxiv.org/abs/2404.04793) | [ICLR 2025 Poster](https://openreview.net/forum?id=9HK2rHNAhd) | promoted | [note](notes/2026/2026-03-25-2404.04793-squeezeattention.md) | accepted batch, main taxonomy |
| 2026-03-25 | [RazorAttention: Efficient KV Cache Compression Through Retrieval Heads](https://arxiv.org/abs/2407.15891) | [ICLR 2025 Poster](https://openreview.net/forum?id=tkiZQlL04w) | promoted | [note](notes/2026/2026-03-25-2407.15891-razorattention.md) | accepted batch, main taxonomy |
| 2026-03-25 | [Palu: KV-Cache Compression with Low-Rank Projection](https://arxiv.org/abs/2407.21118) | [ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/7da6e0e00702c60607a6ae05c802ef85-Abstract-Conference.html) | promoted | [note](notes/2026/2026-03-25-2407.21118-palu.md) | accepted batch, main taxonomy |
| 2026-03-25 | [CAKE: Cascading and Adaptive KV Cache Eviction with Layer Preferences](https://arxiv.org/abs/2503.12491) | [ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/dfae940651f3e690a12e19c874edad7c-Abstract-Conference.html) | promoted | [note](notes/2026/2026-03-25-2503.12491-cake.md) | accepted batch, main taxonomy |
| 2026-03-25 | [KVTuner: Sensitivity-Aware Layer-Wise Mixed-Precision KV Cache Quantization for Efficient and Nearly Lossless LLM Inference](https://arxiv.org/abs/2502.04420) | [ICML 2025](https://proceedings.mlr.press/v267/li25dd.html) | promoted | [note](notes/2026/2026-03-25-2502.04420-kvtuner.md) | accepted batch, main taxonomy |
| 2026-03-25 | [NEO: Saving GPU Memory Crisis with CPU Offloading for Online LLM Inference](https://arxiv.org/abs/2411.01142) | [MLSys 2025](https://proceedings.mlsys.org/paper_files/paper/2025/hash/66a026c0d17040889b50f0dfa650e5e0-Abstract-Conference.html) | promoted | [note](notes/2026/2026-03-25-2411.01142-neo.md) | accepted batch, main taxonomy |
| 2026-03-25 | [Marconi: Prefix Caching for the Era of Hybrid LLMs](https://arxiv.org/abs/2411.19379) | [MLSys 2025](https://proceedings.mlsys.org/paper_files/paper/2025/hash/7c180af017258d239bac6248d1eb26ac-Abstract-Conference.html) | promoted | [note](notes/2026/2026-03-25-2411.19379-marconi.md) | accepted batch, main taxonomy |
| 2026-03-25 | [SpeCache: Speculative Key-Value Caching for Efficient Generation of LLMs](https://arxiv.org/abs/2503.16163) | [ICML 2025](https://proceedings.mlr.press/v267/jie25a.html) | promoted | [note](notes/2026/2026-03-25-2503.16163-specache.md) | accepted batch, main taxonomy |
| 2026-03-25 | [ShadowKV: KV Cache in Shadows for High-Throughput Long-Context LLM Inference](https://arxiv.org/abs/2410.21465) | [ICML 2025](https://proceedings.mlr.press/v267/sun25b.html) | promoted | [note](notes/2026/2026-03-25-2410.21465-shadowkv.md) | accepted batch, main taxonomy |
| 2026-03-25 | [RocketKV: Accelerating Long-Context LLM Inference via Two-Stage KV Cache Compression](https://arxiv.org/abs/2502.14051) | [ICML 2025](https://proceedings.mlr.press/v267/behnam25a.html) | promoted | [note](notes/2026/2026-03-25-2502.14051-rocketkv.md) | accepted batch, main taxonomy |
| 2026-03-25 | [CommVQ: Commutative Vector Quantization for KV Cache Compression](https://arxiv.org/abs/2506.18879) | [ICML 2025](https://proceedings.mlr.press/v267/li25du.html) | promoted | [note](notes/2026/2026-03-25-2506.18879-commvq.md) | accepted batch, main taxonomy |
| 2026-03-25 | [KeyDiff: Key Similarity-Based KV Cache Eviction for Long-Context LLM Inference in Resource-Constrained Environments](https://arxiv.org/abs/2504.15364) | [NeurIPS 2025 Poster](https://openreview.net/forum?id=uBaFH7aQnC) | promoted | [note](notes/2026/2026-03-25-2504.15364-keydiff.md) | accepted batch, main taxonomy |
| 2026-03-25 | [Lookahead Q-Cache: Achieving More Consistent KV Cache Eviction via Pseudo Query](https://arxiv.org/abs/2505.20334) | [EMNLP 2025 Main](https://aclanthology.org/2025.emnlp-main.1732/) | promoted | [note](notes/2026/2026-03-25-2505.20334-lookahead-q-cache.md) | accepted batch, main taxonomy |
| 2026-03-25 | [KVCache Cache in the Wild: Characterizing and Optimizing KVCache Cache at a Large Cloud Provider](https://arxiv.org/abs/2506.02634) | [USENIX ATC 2025](https://www.usenix.org/conference/atc25/presentation/wang-jiahao) | promoted | [note](notes/2026/2026-03-25-2506.02634-kvcache-cache-in-the-wild.md) | accepted batch, main taxonomy |
| 2026-03-25 | [TailorKV: A Hybrid Framework for Long-Context Inference via Tailored KV Cache Optimization](https://arxiv.org/abs/2505.19586) | [Findings of ACL 2025](https://aclanthology.org/2025.findings-acl.1043/) | promoted | [note](notes/2026/2026-03-25-2505.19586-tailorkv.md) | accepted batch, main taxonomy |
| 2026-03-25 | [XQuant: Achieving Ultra-Low Bit KV Cache Quantization with Cross-Layer Compression](https://arxiv.org/abs/2510.11236) | [EMNLP 2025 Main](https://aclanthology.org/2025.emnlp-main.494/) | promoted | [note](notes/2026/2026-03-25-2510.11236-xquant.md) | accepted batch, main taxonomy |

## Current Tracking Queue

Window reviewed in this batch: `2026-02-25` to `2026-03-25`.

| Added | Paper | ArXiv | Category Guess | Status | Note | Why It Matters |
| --- | --- | --- | --- | --- | --- | --- |
| 2026-03-25 | DualPath: Breaking the Storage Bandwidth Bottleneck in Agentic LLM Inference | [2602.21548](https://arxiv.org/abs/2602.21548) | offloading_and_disaggregated_serving | promoted | [note](notes/2026/2026-03-25-2602.21548-dualpath.md) | 把 KV 瓶颈建模为分离式 serving 的存储带宽不对称问题。 |
| 2026-03-25 | KEEP: A KV-Cache-Centric Memory Management System for Efficient Embodied Planning | [2602.23592](https://arxiv.org/abs/2602.23592) | memory_management_and_cache_layout | promoted | [note](notes/2026/2026-03-25-2602.23592-keep.md) | 用 KV 作为动态记忆底座，优化重算、加载和 TTFT。 |
| 2026-03-25 | KVSlimmer: Theoretical Insights and Practical Optimizations for Asymmetric KV Merging | [2603.00907](https://arxiv.org/abs/2603.00907) | compression_and_sparsification | promoted | [note](notes/2026/2026-03-25-2603.00907-kvslimmer.md) | 给 KV merging 补上理论解释和更低开销的闭式解。 |
| 2026-03-25 | HGCA: Hybrid GPU-CPU Attention for Long Context LLM Inference | [2507.03153](https://arxiv.org/abs/2507.03153) | offloading_and_disaggregated_serving | promoted | [note](notes/2026/2026-03-25-2507.03153-hgca.md) | 已提升到主 taxonomy；补上 hybrid GPU-CPU attention 这条系统路线。 |
| 2026-03-25 | Understanding the Physics of Key-Value Cache Compression for LLMs through Attention Dynamics | [2603.01426](https://arxiv.org/abs/2603.01426) | compression_and_sparsification | noted | [note](notes/2026/2026-03-25-2603.01426-kv-physics.md) | 解释压缩何时开始伤语义可达性和安全性。 |
| 2026-03-25 | Multi-Head Low-Rank Attention | [2603.02188](https://arxiv.org/abs/2603.02188) | system_and_kernel_codesign | noted | [note](notes/2026/2026-03-25-2603.02188-mlra.md) | 为 TP 解码重设计 latent attention 的可分片性。 |

## Index Schema

| Added | Paper | ArXiv | Category Guess | Status | Note | Why It Matters |
| --- | --- | --- | --- | --- | --- | --- |
| YYYY-MM-DD | Title | 1234.56789 | quantization / eviction / serving | watching | `notes/2026/...` | one-line reason |

Notes can additionally record:

- `Authors`
- `Affiliations`
- `Comment` such as accepted venue if publicly shown on arXiv/OpenReview

## Suggested Routine

When a new paper appears:

1. Add it here first
2. Write a 5-minute note
3. Decide whether it is truly KV-cache-centric
4. If yes, promote it to the main list and keep the note link

## Notes

- Do not try to fully summarize the whole paper here
- Keep each note short and decision-oriented
- Prefer recording why the paper matters for KV cache over generic paper summaries
