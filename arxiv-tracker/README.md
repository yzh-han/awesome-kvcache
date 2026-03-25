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

## Current Tracking Queue

Window reviewed in this batch: `2026-02-25` to `2026-03-25`.

| Added | Paper | ArXiv | Category Guess | Status | Note | Why It Matters |
| --- | --- | --- | --- | --- | --- | --- |
| 2026-03-25 | DualPath: Breaking the Storage Bandwidth Bottleneck in Agentic LLM Inference | [2602.21548](https://arxiv.org/abs/2602.21548) | offloading_and_disaggregated_serving | promoted | [note](notes/2026/2026-03-25-2602.21548-dualpath.md) | 把 KV 瓶颈建模为分离式 serving 的存储带宽不对称问题。 |
| 2026-03-25 | KEEP: A KV-Cache-Centric Memory Management System for Efficient Embodied Planning | [2602.23592](https://arxiv.org/abs/2602.23592) | memory_management_and_cache_layout | promoted | [note](notes/2026/2026-03-25-2602.23592-keep.md) | 用 KV 作为动态记忆底座，优化重算、加载和 TTFT。 |
| 2026-03-25 | KVSlimmer: Theoretical Insights and Practical Optimizations for Asymmetric KV Merging | [2603.00907](https://arxiv.org/abs/2603.00907) | compression_and_sparsification | promoted | [note](notes/2026/2026-03-25-2603.00907-kvslimmer.md) | 给 KV merging 补上理论解释和更低开销的闭式解。 |
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
