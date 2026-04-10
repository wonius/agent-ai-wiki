---
title: summaries/skillrouter-2603-22455
type: summary
source_url: https://arxiv.org/abs/2603.22455
source_type: paper
date: 2026-03-28
ingested: 2026-04-10
tags: [skill-routing, retrieve-and-rerank, llm-agent, bi-encoder, cross-encoder]
---

# SkillRouter: Skill Routing for LLM Agents at Scale

**Source**: [YanZhao Zheng et al., Alibaba — arXiv 2603.22455](https://arxiv.org/abs/2603.22455) · 2026-03-28

## Key takeaways

- **全技能正文是关键路由信号**：移除 body 导致所有基线 Hit@1 下降 31-44pp（80K 技能池）
- 1.2B 模型（SR-Emb-0.6B × SR-Rank-0.6B）达到 74.0% Hit@1，超越 16B 基线，推理快 5.8×
- False Negative Filtering 移除 ~10% 假负例（功能等价的近重复技能），贡献 +4.0pp
- Listwise Reranking Loss 贡献 +30.7pp vs pointwise BCE（43.3% → 74.0%）
- 路由增益可迁移到下游编码 Agent 任务成功率 +1.78pp（top-1）

## Core claims

SkillRouter 研究当 LLM Agent 技能生态扩展到数以万计时，如何在推理时高效准确地路由到相关技能。两阶段 Retrieve-and-Rerank 流水线：Bi-encoder 检索 Top-20 + Cross-encoder listwise 重排。关键发现是现有系统隐藏技能 body（只暴露 metadata）的设计导致 31-44pp 精度损失——body 中的实现细节才是关键路由信号。三层 False Negative Filtering（name 去重 + trigram Jaccard + 嵌入相似度）处理同质化技能池的近重复问题。

## Notable quotes

> "Hidden-Body Asymmetry：路由系统可以看到完整技能正文，但下游 Agent 通常只看到元数据——这一设计选择此前未经实测验证"

> "Fine-tuning 比 Scale 更有效：任务特定训练数据 + 负采样策略可以弥补 13× 规模差距"

## Concepts introduced / referenced

- [[技能路由]] — 核心问题：大规模技能池中的精准定位
- [[Retrieve-and-Rerank]] — 两阶段排序范式
- [[Progressive Disclosure]] — 隐藏技能 body 以减少上下文的策略，AgentFold 和 SkillRouter 均挑战这一范式
