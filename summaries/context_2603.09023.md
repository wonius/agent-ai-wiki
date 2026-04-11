---
title: summaries/context_2603.09023
type: summary
source_url: https://arxiv.org/abs/2603.09023
source_type: paper
date: 2026-03-09
ingested: 2026-04-11
tags: [LLM Context, Demand Paging, Virtual Memory, Pichay, Memory Hierarchy]
---

# The Missing Memory Hierarchy: Demand Paging for LLM Context Windows

**Source**: [arXiv 2603.09023](https://arxiv.org/abs/2603.09023) · 2026-03-09

## Key takeaways

- Context window is L1 cache, not memory—no L2, no virtual memory, no paging
- 21.8% structural waste in production: unused tool schemas (11%), duplicated (2.2%), stale tool results (8.7%)
- Pichay: demand paging system as transparent proxy
- Offline fault rate: 0.0254%; live deployment reduces context by up to 93%
- Four-level memory hierarchy: L1 (generation), L2 (working set), L3 (session history), L4 (cross-session)

## Core claims

Context window of LLM is not memory—it is L1 cache. Every tool definition, system prompt, and stale tool result occupies context for session lifetime. Presents Pichay, a demand paging system implemented as transparent proxy between client and inference API. Distinguishes garbage collection (removing ephemeral output) from paging (evicting addressable content). Detects page faults when model re-requests evicted content. Proposes full memory hierarchy for LLM systems, identifying context limits, attention degradation, cost scaling as virtual memory problems.

## Notable quotes

> "The context window of a large language model is not memory. It is L1 cache"

## Concepts introduced / referenced

- [[Pichay]]
- [[需求分页]]
- [[虚拟内存]]
- [[工作集理论]]
- [[内存层次结构]]
- [[L1/L2/L3/L4]]

## Sources

- [[wiki/raw/papers/context_2603.09023_extracted.json]] — 2026-04-11 提取数据