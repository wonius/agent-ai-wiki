---
title: summaries/convsearch-r1-2503-21278
type: summary
source_url: https://arxiv.org/abs/2503.21278
source_type: paper
date: 2025-03-25
ingested: 2026-04-10
tags: [conversational-search, query-reformulation, reinforcement-learning, grpo, self-distillation]
---

# ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via RL

**Source**: [arXiv 2503.21278](https://arxiv.org/abs/2503.21278) · 2025-03-25

## Key takeaways

- 两阶段框架（SDPWU + GRPO/RIRS）：3B 小模型通过自蒸馏而非外部蒸馏学会推理式查询改写
- 无外部监督数据（无人工 rewrite，无强模型蒸馏），TopiOCQA MRR@3 达 50.5，超越所有 7B 基线
- RIRS（排序激励奖励整形）piecewise 线性奖励解决 MRR/NDCG 的 reward sparsity
- 核心洞察：推理能力不等于改写能力，直接蒸馏推理模型（DS-R1-7B）反而比 Raw 更差

## Core claims

对话搜索中用户 query 依赖上下文（代词、指代），需要改写为 context-independent 形式。现有方法依赖外部强模型蒸馏或人工标注，成本高且泛化差。ConvSearch-R1 的两阶段方案：Stage 1 SDPWU 用 few-shot prompt 让小模型自蒸馏（过滤条件：gold passage 必须 rank=1）；Stage 2 GRPO + RIRS 用 piecewise 线性奖励函数优化，rank 1-10 获 1.0-2.0 奖励，rank 11-100 获 0-1.0 奖励，解决 reward sparsity。

## Notable quotes

> "直接使用推理模型（DS-R1-Distill-Qwen-7B）反而比 Raw 更差——推理能力不等于改写能力"

## Concepts introduced / referenced

- [[Query 重写]] — 对话搜索核心挑战
- [[检索增强生成]] — ConvSearch-R1 的下游任务
- [[上下文饱和]] — 与 AgentFold/ACON 处理同一问题的不同视角
