---
title: summaries/context_2407.09450
type: summary
source_url: https://arxiv.org/abs/2407.09450
source_type: paper
date: 2024-07-12
ingested: 2026-04-11
tags: [LLM, Episodic Memory, Long Context, Event Segmentation, EM-LLM]
---

# EM-LLM: Human-Inspired Episodic Memory for Infinite Context LLMs

**Source**: [arXiv 2407.09450](https://arxiv.org/abs/2407.09450) · 2024-07-12

## Key takeaways

- Integrates human episodic memory and event cognition into LLMs without fine-tuning
- Enables practically infinite context lengths while maintaining computational efficiency
- Organizes tokens into coherent episodic events using Bayesian surprise + graph-theoretic boundary refinement
- Two-stage memory retrieval: similarity-based + temporally contiguous
- Outperforms InfLLM and RAG on LongBench and ∞-Bench
- Successfully performs retrieval across 10 million tokens

## Core claims

EM-LLM addresses LLM limitations in processing extensive contexts by taking inspiration from human episodic memory. The approach segments sequences into episodic events based on surprise (prediction errors) and refines boundaries using graph-theoretic metrics. Memory retrieval combines similarity-based access with temporal contiguity mechanisms. The event segmentation correlates with human-perceived events, suggesting parallels between artificial and biological memory systems.

## Notable quotes

> "Surprise refers to moments when the brain's predictions about incoming sensory information are significantly violated"

## Concepts introduced / referenced

- [[EM-LLM]]
- [[情景记忆]]
- [[事件分割]]
- [[贝叶斯惊喜]]
- [[InfLLM]] (compared)
- [[RAG]] (compared)

## Sources

- [[wiki/raw/papers/context_2407.09450_extracted.json]] — 2026-04-11 提取数据