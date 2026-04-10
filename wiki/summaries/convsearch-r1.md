---
title: ConvSearch-R1
type: summary
created: 2026-04-07
updated: 2026-04-09
sources: ["raw/papers/ConvSearch-R1_2503.21278.pdf", "raw/papers/ConvSearch-R1_2503.21278_深度阅读报告.md"]
tags: [Conversational Search, Query Reformulation, Reinforcement Learning, GRPO, Information Retrieval, EMNLP 2025]
---

# ConvSearch-R1

**ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via Reinforcement Learning**
来源：EMNLP 2025 | 作者：复旦大学 + ByteDance

## 一句话概括

通过"自蒸馏（SDPWU）+ 检索引导的强化学习（GRPO/RIRS）"两阶段框架，让 3B 小模型学会推理式查询改写，无需外部监督数据即刷新 SOTA。

## 核心问题

对话搜索中用户 query 依赖上下文（代词、指代、省略），需要改写成 context-independent 形式。现有方法依赖外部强模型蒸馏或人工标注，成本高且泛化差。

## 核心方法

### Stage 1: Self-Driven Policy Warm-Up (SDPWU)
- 用 few-shot prompt 让 3B 模型自我蒸馏
- 过滤条件：gold passage 必须 rank=1
- 产出：格式稳定的 SFT 模型

### Stage 2: Retrieval-Guided RL (GRPO + RIRS)
- **Rank-Incentive Reward Shaping (RIRS)**：piecewise 线性奖励
  - rank 1-10：奖励 1.0-2.0
  - rank 11-100：奖励 0-1.0
  - format 不合规：-0.1 惩罚
- GRPO 优化，无需 reward model
- 解决直接用 MRR/NDCG 作 reward 导致的 reward sparsity 问题

## 关键结果

| 模型 | 数据集 | MRR@3 | NDCG@3 | R@10 |
|------|--------|-------|--------|------|
| ConvSearch-R1 (Llama3.2-3B) | TopiOCQA (ANCE) | **50.5** | 50.1 | 72.0 |
| ConvSearch-R1 (Qwen2.5-3B) | TopiOCQA (ANCE) | **51.4** | 51.3 | 72.0 |
| ConvSearch-R1 (Llama3.2-3B) | QReCC (ANCE) | 50.2 | 48.1 | 70.6 |
| SOTA (ADACQR) | TopiOCQA (ANCE) | 38.5 | 37.6 | 61.6 |
| SOTA (CHIQ-Fusion) | QReCC (ANCE) | 47.2 | 44.2 | 70.7 |

- 3B 模型超越所有 7B 竞品
- 无外部监督数据（No External Distillation, No Human Rewrite）
- 跨数据集泛化能力强（TREC CAsT 2019/2020/2021 同样 SOTA）

## Core claims

1. **推理能力可以通过自蒸馏获得**：无需外部强模型蒸馏
2. **RIRS 解决 reward sparsity**：piecewise 奖励比直接用 MRR 高 3 倍以上
3. **模型规模越小收益越大**：0.5B 就能超越 SOTA

## Concepts introduced

- [[Rank-Incentive Reward Shaping]] — 分段线性奖励函数
- [[Self-Driven Policy Warm-Up]] — 自蒸馏 warm-up
- [[Conversational Query Reformulation]] — 会话查询改写

## 可复现要素

- 基座：Llama3.2-3B / Qwen2.5-3B（开源）
- SFT：batch=64, seq_len=3072, epoch=2, lr=1e-5
- RL：batch=128, prompt_len=1536, lr=1e-6, clip=0.2, KL=0.001, rollout=8
- 训练框架：verl

## 与 ACON / AgentFold 的关联

ConvSearch-R1 关注**检索前的 query 改写**（信息获取优化），ACON/AgentFold 关注**上下文压缩**（信息量管理），两者是正交的优化维度。

## 相关论文

- [[summaries/acon]] — 上下文压缩优化
- [[summaries/agentfold]] — 主动上下文折叠
- [[summaries/skillrouter]] — 技能路由
