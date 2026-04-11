---
title: summaries/context_2506.08184
type: summary
source_url: https://arxiv.org/abs/2506.08184
source_type: paper
date: 2025-06-09
ingested: 2026-04-11
tags: [LLM, Proactive Interference, Working Memory, Retrieval, PI-LLM]
---

# Unable to Forget: Proactive Interference Reveals Working Memory Limits in LLMs

**Source**: [arXiv 2506.08184](https://arxiv.org/abs/2506.08184) · 2025-06-09

## Key takeaways

- LLM retrieval accuracy declines log-linearly toward zero as interference accumulates
- Interfering information—semantically similar distractors—independently impacts retrieval
- Errors arise from retrieving previously overwritten values
- Prompt engineering (e.g., "ignore earlier input") yields limited success
- Larger models achieve higher Interference Endurance Score (IES)
- Context window length has no significant effect on interference resistance
- Reveals working memory bottleneck beyond mere context access

## Core claims

This work adapts the proactive interference (PI) paradigm from cognitive science, where earlier information disrupts recall of newer updates. Introduces PI-LLM evaluation: sequentially streams semantically related key-value updates and queries only final values. Despite target being positioned just before query, retrieval accuracy declines log-linearly. Reveals fundamental constraint: LLMs cannot reliably suppress competing cues—working memory bottleneck beyond context length.

## Notable quotes

> "This bottleneck mirrors the unitary working memory limit found in humans"

## Concepts introduced / referenced

- [[主动干扰]]
- [[工作记忆]]
- [[PI-LLM]]
- [[干扰耐受度]]

## Incomplete sections

深度阅读报告待完成

## Sources

- [[wiki/raw/papers/context_2506.08184_extracted.json]] — 2026-04-11 提取数据