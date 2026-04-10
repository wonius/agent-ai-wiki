---
title: summaries/skillrouter
type: summary
source_url: https://arxiv.org/abs/2603.22455
source_type: paper
date: 2026-03-24
ingested: 2026-04-09
tags: [Skill Routing, LLM Agent, Retrieve-and-Rerank, Large-Scale Skills, Full-Text Retrieval]
---

# SkillRouter: Skill Routing for LLM Agents at Scale

**Source**: [Zheng et al. (Alibaba)](https://arxiv.org/abs/2603.22455) · 2026-03-24

## Key takeaways

- **Full skill body is critical routing signal**: On a ∼80K-skill pool, removing skill body (keeping only name+description) causes 31–44pp Hit@1 drops across sparse, dense, and reranking baselines — not a minor refinement but a critical signal
- **Compact 1.2B model outperforms 16B baseline**: Task-specific training (false negative filtering + listwise reranking) compensates for a 13× parameter gap; SKILLRouter achieves 74.0% Hit@1 vs. 68.0% for the strongest 16B pipeline
- **False negative filtering is essential**: Near-duplicate skills (same capability implemented independently by different authors) corrupt contrastive learning; three-layer filtering (name dedup + trigram Jaccard > 0.6 + embedding similarity > 0.92) contributes +4.0pp Hit@1
- **Listwise reranking is decisive**: When top-20 candidates are all topically plausible, pointwise BCE collapses to 43.3% Hit@1; listwise cross-entropy reaches 74.0% (+30.7pp)
- **Routing gains transfer to downstream agents**: SKILLRouter improves average task success by +1.78pp (top-1) and +2.33pp (top-10) across four coding agents, with larger gains for more capable agents (+3.22pp for Claude Sonnet/Opus)
- **5.8× serving speedup**: 1.2B pipeline at 495.8ms median latency vs. 16B baseline, with 15.8% less GPU memory

## Core claims

SkillRouter addresses the skill-routing problem in large agent skill registries (∼80K skills): given a user task, retrieve the relevant skill set before downstream planning/execution. The paper's central empirical finding is that **full skill text (body) is a critical routing signal** rather than a minor metadata refinement — a 31–44pp Hit@1 collapse when body is removed across all baseline families. Motivated by this, the authors build SKILLRouter, a compact 1.2B full-text retrieve-and-rerank pipeline (0.6B bi-encoder + 0.6B cross-encoder reranker) using Qwen3-Emb-0.6B and Qwen3-Rank-0.6B. Two training adaptations are specifically necessary for homogeneous skill pools: false-negative filtering (removes ∼10% near-duplicate negatives) and listwise reranking loss (resolves fine-grained candidate competition).

## Notable quotes

> "Current agent stacks often rely on progressive disclosure, exposing only skill names and descriptions while hiding the full implementation body... hiding the skill body causes a 31–44 percentage point drop in routing accuracy." — Zheng et al., 2026

> "Fine-tuning is more valuable than scale alone: SR-Emb-0.6B (0.6B) surpasses Qwen3-Emb-8B (8B) despite a 13× parameter gap." — Zheng et al., 2026

## Key results

| Pipeline | Params | A-Hit@1 | Serving Latency |
|----------|--------|---------|-----------------|
| Qwen3-Emb-8B×Qwen3-Rank-8B (base) | 16B | 68.0% | baseline |
| **SR-Emb-0.6B×SR-Rank-0.6B (ours)** | **1.2B** | **74.0%** | **5.8× faster** |
| SR-Emb-8B×SR-Rank-8B (ours, 8B) | 8B | 76.0% | — |

## Concepts introduced / referenced

- [[AgentFold]] — Agent 上下文管理（两者都研究如何在信息爆炸场景下保持关键信号）
- [[ConvSearch-R1]] — 检索增强式对话搜索
- [[ACON]] — 自适应对比学习

## Sources

- [[raw/papers/SkillRouter_2603.22455_深度阅读报告]] — full 10-section deep reading report
