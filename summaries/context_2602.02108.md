---
title: summaries/context_2602.02108
type: summary
source_url: https://arxiv.org/abs/2602.02108
source_type: paper
date: 2026-02-02
ingested: 2026-04-11
tags: [LLM Training, Long Context, Memory Efficiency, OOMB, Chunk-Recurrent]
---

# Out of the Memory Barrier: A Highly Memory Efficient Training System for LLMs

**Source**: [arXiv 2602.02108](https://arxiv.org/abs/2602.02108) · 2026-02-02

## Key takeaways

- OOMB: chunk-recurrent training with on-the-fly activation recomputation
- O(1) activation memory footprint regardless of sequence length
- For every 10K additional tokens, only 10MB memory overhead for Qwen2.5-7B
- Enables 4M-token context training on single H200 GPU
- KV cache optimizations: paged memory, async CPU offloading, page-level sparse attention

## Core claims

Training LLMs on long contexts is severely constrained by GPU memory overhead, not training time. The primary culprits are activations whose memory scales linearly with sequence length. OOMB employs chunk-recurrent framework with activation recomputation, maintaining constant activation memory (O(1)). Shifts bottleneck to KV cache, managed through paged memory manager, asynchronous CPU offloading, and page-level sparse attention. Achieves single-GPU training with 4M-token context that would otherwise require large cluster with context parallelism.

## Concepts introduced / referenced

- [[OOMB]]
- [[长上下文训练]]
- [[Chunk-Recurrent]]
- [[KV Cache]]
- [[激活重计算]]
- [[分页内存管理]]

## Sources

- [[wiki/raw/papers/context_2602.02108_extracted.json]] — 2026-04-11 提取数据