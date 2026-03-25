# Reading Roadmap

This roadmap is meant for quickly onboarding yourself into KV-cache research without reading papers in a random order.

## Track 1: Build the Mental Model First

Read these first:

1. [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/abs/2309.06180)
2. [Efficient Streaming Language Models with Attention Sinks](https://arxiv.org/abs/2309.17453)
3. [H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models](https://proceedings.neurips.cc/paper_files/paper/2023/hash/6ceefa7b15572587b78ecfcebb2827f8-Abstract-Conference.html)

Goal:

- Understand why KV cache becomes the dominant memory bottleneck
- Separate `prefill` from `decode`
- Separate `layout` problems from `selection` problems

## Track 2: Compression and Eviction

Recommended order:

1. H2O
2. SnapKV
3. MiniCache
4. PyramidKV
5. CAKE

Questions to keep in mind:

- Is the method deciding what to keep, or how to represent what is kept?
- Is the budget uniform across layers?
- Does the method mainly help prefill, decode, or both?

## Track 3: Quantization

Recommended order:

1. KIVI
2. KVQuant
3. QServe

Questions to keep in mind:

- Why are keys and values quantized differently?
- Which axis is used for quantization?
- Does the paper only save memory, or also improve actual end-to-end serving throughput?

## Track 4: Reuse and Serving Systems

Recommended order:

1. PagedAttention / vLLM
2. Preble
3. LMCache
4. FlowKV

Questions to keep in mind:

- Is the gain from sharing, scheduling, placement, or transfer?
- Does the system only reuse prefixes, or more general repeated text spans?
- How does the system behave under multi-tenant or distributed workloads?

## A Simple 3-Layer View

You can mentally organize the space into three layers:

1. **Algorithm layer**
   Compression, quantization, eviction, streaming
2. **Runtime layer**
   Paging, sharing, scheduling, prefix reuse
3. **Infrastructure layer**
   CPU/GPU tiering, remote transfer, disaggregated serving

Most new papers are easier to place once you ask which of these three layers carries the main novelty.

