---
title: ConvSearch-R1 论文摘要
type: summary
created: 2026-04-07
updated: 2026-04-07
sources: ["raw/papers/ConvSearch-R1_2503.21278.pdf", "raw/papers/ConvSearch-R1_深度阅读报告.md"]
tags: [Conversational Search, Query Reformulation, Reinforcement Learning, Information Retrieval, EMNLP 2025]
---

# ConvSearch-R1 论文摘要

## 一句话概括

ConvSearch-R1 是首个**完全摆脱外部重写监督**的会话搜索 Query 重写框架，通过**检索信号驱动的强化学习**直接优化重写策略，3B 小模型在 TopiOCQA 上超越 SOTA 超过 10%。

## 核心问题

会话搜索中用户 query 通常包含**歧义、省略、指代**（如"它"、"那个"），不能直接用于检索。

现有 CQR 方法的两个关键缺陷：
1. **过度依赖外部监督**：需要人工标注或 GPT-4 生成重写结果，成本高昂
2. **与下游检索器对齐不足**：重写模型和检索器各自为战

## 核心方法：ConvSearch-R1

### Stage 1: Self-Driven Policy Warm-Up (SDPWU)

解决 RL 冷启动：用检索信号本身筛选自生成的高质量数据，无需外部 LLM 蒸馏。

流程：few-shot 生成 → 格式过滤 → 筛选 gold passage 排第1的样本 → SFT

### Stage 2: Retrieval-Guided Reinforcement Learning (RGRL)

**Rank-Incentive Reward Shaping (RIRS)**：设计分段奖励函数解决传统检索指标（MRR@3、NDCG@3）的奖励稀疏问题。排名 1-10 给予高奖励，排名 11-100 给予次级奖励。

采用 **GRPO** 算法，无需 reward model 或 value model。

## 关键结果

- TopiOCQA：>10% 超越 SOTA
- 3B 模型（Llama3.2-3B / Qwen2.5-3B）超越依赖外部 LLM 监督的 7B 竞品
- 无需任何外部重写监督、无需训练检索器

## 相关信息

- 论文：https://arxiv.org/abs/2505.15776
- 作者：Changtai Zhu, Siyin Wang, Ruijun Feng, Kai Song, Xipeng Qiu（复旦大学）
- 发布：EMNLP 2025 Main Conference
