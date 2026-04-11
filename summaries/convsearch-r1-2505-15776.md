---
title: summaries/convsearch-r1-2505-15776
type: summary
source_url: https://arxiv.org/abs/2505.15776
source_type: paper
date: 2025-05-21
ingested: 2026-04-11
tags: [LLM Agent, Conversational Search, Query Reformulation, Reinforcement Learning]
---

# ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via Reinforcement Learning

**Source**: [arXiv 2505.15776](https://arxiv.org/abs/2505.15776) · 2025-05-21

## Key takeaways

- Self-driven conversational query reformulation using RL (no external labeled data)
- Uses Self-Driven Policy Warm-Up (SDPWU) to solve cold-start problem
- Group Relative Policy Optimization (GRPO) with Rank-Incentive Reward Shaping (RIRS)
- 3B model (Llama3.2-3B / Qwen2.5-3B) outperforms 7B models relying on external supervision
- Average improvement >10% on TopiOCQA dataset

## Core claims

ConvSearch-R1 addresses conversational search query reformulation entirely through self-supervised RL. The two-stage framework (SDPWU + GRPO) eliminates dependence on external rewrite supervision while achieving state-of-the-art results — with a 3B model surpassing 7B models that use labeled rewrite data. Rank-Incentive Reward Shaping solves the sparse reward problem of traditional IR metrics.

## Notable quotes

> "Self-Driven Policy Warm-Up (SDPWU) leverages the model's few-shot reasoning capability combined with retrieval ranking signals for self-distillation"

> "Rank-Incentive Reward Shaping transforms sparse/metric-based rewards into smoother learning signals by rewarding relative rank improvements"

> "3B model achieves >10.3% average improvement over 7B models on TopiOCQA"

## Concepts introduced / referenced

- [[Query 重写]]
- [[检索增强生成]]
- [[自驱动学习]]
- [[小模型策略]]
- [[GRPO]] (Group Relative Policy Optimization)
- [[排名激励奖励塑形]]
- [[SDPWU]]

## Relationship to existing concepts

- Shares theme with [[AgentFold]] and [[SkillRouter]] on "small model + good training paradigm > large model"
- [[上下文管理]] angle: query reformulation is a form of proactive context clarification

## Sources

- [[raw/papers/ConvSearch-R1_2505.15776_深度阅读报告.md]] — 2026-04-10 full-depth analysis report
