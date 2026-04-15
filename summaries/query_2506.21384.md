---
title: summaries/query_2506.21384
type: summary
source_url: https://arxiv.org/abs/2506.21384
source_type: paper
date: 2025-06-26
ingested: 2026-04-11
tags: [RAG, Query Understanding, Live Retrieval, Multi-Intent]
---

# Leveraging LLM-Assisted Query Understanding for Live Retrieval-Augmented Generation

**Source**: [arXiv 2506.21384](https://arxiv.org/abs/2506.21384) · 2025-06-26

## Key takeaways

- Proposes Omni-RAG framework for robust live RAG systems
- Handles noisy, ambiguous, multi-intent user queries
- Three key modules: Deep Query Understanding, Intent-Aware Retrieval, Reranking
- Uses LLM for query denoising and decomposition
- Addresses SIGIR 2025 LiveRAG Challenge requirements

## Core claims

Real-world RAG systems struggle with complex inputs (noisy, ambiguous, multi-intent). Omni-RAG preprocesses queries through LLM-assisted understanding: denoising spelling errors, decomposing multi-intent queries, intent-aware retrieval, and reranking before generation.

## Notable quotes

> Real-world live RAG systems face significant challenges when processing user queries that are often noisy, ambiguous, and contain multiple intents.

## Concepts introduced / referenced

- [[实时RAG]]
- [[Omni-RAG]]
- [[查询去噪]]
- [[意图分解]]
- [[多意图查询]]

## Sources

- [[raw/papers/query_2506.21384_深度阅读报告.md]]