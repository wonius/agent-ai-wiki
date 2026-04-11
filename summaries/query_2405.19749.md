---
title: summaries/query_2405.19749
type: summary
source_url: https://arxiv.org/abs/2405.19749
source_type: paper
date: 2024-05-30
ingested: 2026-04-11
tags: [Query Recommendation, LLM, Search, Cold Start]
---

# Generating Query Recommendations via LLMs

**Source**: [arXiv 2405.19749](https://arxiv.org/abs/2405.19749) · 2024-05-30

## Key takeaways

- Proposes Generative Query Recommendation (GQR) framing query recommendation as generative task
- Uses LLM as foundation without requiring training or fine-tuning
- RA-GQR retrieves similar queries from logs for dynamic prompt composition
- Achieves state-of-the-art NDCG@10 and clarity score against commercial search engines
- Works in cold start scenarios without query logs

## Core claims

Traditional query recommendation requires large document collections and query logs, expensive to collect. GQR leverages LLM's inherent knowledge to generate recommendations, with a simple prompt enabling the task even from single example. The retriever-augmented version further improves by incorporating historical query data.

## Notable quotes

> Query logs are expensive to collect and maintain and require complex and time-consuming cascading pipelines.

## Concepts introduced / referenced

- [[查询推荐]]
- [[生成式模型]]
- [[冷启动问题]]
- [[RA-GQR]]

## Sources

- [[raw/papers/query_2405.19749_深度阅读报告.md]]