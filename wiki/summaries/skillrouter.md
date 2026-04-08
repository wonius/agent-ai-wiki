---
title: SkillRouter 论文摘要
type: summary
created: 2026-04-08
sources: ["raw/papers/skillrouter.md"]
tags: [Skill Routing, LLM Agent, Retrieve-and-Rerank, Large-Scale Skills]
---

# SkillRouter 论文摘要

## 一句话概括

SkillRouter 针对大规模技能库（~80K）的技能路由问题，提出基于全文检索+重排的 1.2B 参数管道，在隐藏技能体导致精度下降 31-44 pp 的情况下，逆势达到 74.0% Hit@1，且比最强基线快 5.8×、参数量少 13×。

## 核心问题

LLM Agent 的技能生态扩展到数万个技能，但 Progressive Disclosure 策略（只暴露技能名称/描述，隐藏实现体）导致路由精度大幅下降。在 SkillsBench（约80K候选）上，这一精度损失高达 **31-44 个百分点**，说明技能体全文是远比元数据更关键的路由信号。

## 核心解法

**两阶段 Retrieve-and-Rerank 管道：**

1. **稀疏检索**：BM25 从 80K 候选中快速召回 Top-K（~100-200）
2. **重排**：1.2B Reranker，将技能体全文作为关键信号重新排序

关键发现：技能体全文是路由的关键信号，而非边缘优化。1.2B 小模型通过专项训练在特定任务上超越大模型。

## 关键结果

- Hit@1：74.0%（最强基线）
- 参数量：1.2B（比最强基线少 13×）
- 推理速度：比最强基线快 5.8×
- 路由增益可传导至 coding agent 端到端任务成功率

## 相关研究

- [[SkillBench]] — 大规模技能路由评测基准
- [[AgentFold]] — Agent 上下文管理（对比：两者都研究如何在信息爆炸场景下保持关键信号）
