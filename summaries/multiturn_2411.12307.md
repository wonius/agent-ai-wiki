---
title: summaries/multiturn_2411.12307
type: summary
source_url: https://arxiv.org/abs/2411.12307
source_type: paper
date: 2024-11-19
ingested: 2026-04-11
tags: [Multi-turn Intent Classification, Symbol Tuning, C-LARA, Production Dialog]
---

# Balancing Accuracy and Efficiency in Multi-Turn Intent Classification for LLM-Powered Dialog Systems

**Source**: [arXiv 2411.12307](https://arxiv.org/abs/2411.12307) · 2024-11-19

## Key takeaways

- Symbol Tuning: 简化冗长的 intent label 提升 5.09% 分类准确率
- C-LARA:  Consistency-aware Linguistics Adaptive Retrieval Augmentation 框架
- 减少 40% 标注成本，实现低资源多语言 industrial 部署
- 在生产对话系统中平衡准确率与效率

## Core claims

该论文针对生产级对话系统中的多轮意图分类提出两大创新：(1) Symbol Tuning - 将冗长的 intent label（如"Request to Cancel Order"）压缩为简洁形式（如"Cancel Order"），减少生成 token 数从而提升性能；(2) C-LARA - 使用 LLM 进行数据增强和伪标签生成，创建合成多轮对话，并利用自洽性和自适应检索增强来优化标注质量。最终可微调小型高效模型用于部署。

## Concepts introduced / referenced

- [[多轮意图分类]]
- [[Symbol Tuning]]
- [[C-LARA]]
- [[伪标签]]
- [[生产对话系统]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2411.12307_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告