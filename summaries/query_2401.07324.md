---
title: summaries/query_2401.07324
type: summary
source_url: https://arxiv.org/abs/2401.07324
source_type: paper
date: 2024-01-14
ingested: 2026-04-11
tags: [LLM Agent, Multi-Agent, Tool Learning, Small Models]
---

# Small LLMs Are Weak Tool Learners: A Multi-LLM Agent (α-UMi)

**Source**: [arXiv 2401.07324](https://arxiv.org/abs/2401.07324) · 2024-01-14

## Key takeaways

- Proposes α-UMi framework that decomposes tool learning into planner, caller, and summarizer
- Each component implemented by a dedicated LLM focusing on specific capability
- Two-stage training paradigm: global pre-training + local fine-tuning
- Outperforms single-LLM approach across various model and data sizes
- Enables smaller LLMs to achieve competitive tool learning performance

## Core claims

Small LLMs are weak tool learners because tool learning demands diverse capabilities (task planning, tool invocation, result summarization) that stress different aspects of LLMs. The multi-LLM framework addresses this by assigning each capability to a specialized model, trained with a global-to-local progressive fine-tuning strategy (GLPFT).

## Notable quotes

> The challenge of tool use demands that LLMs not only understand user queries and generate answers accurately but also excel in task planning, tool invocation, and result summarization.

## Concepts introduced / referenced

- [[工具学习]]
- [[多智能体]]
- [[α-UMi]]
- [[GLPFT]]
- [[规划器-调用器-总结器架构]]

## Sources

- [[raw/papers/query_2401.07324_深度阅读报告.md]]