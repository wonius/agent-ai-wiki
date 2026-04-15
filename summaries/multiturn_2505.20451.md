---
title: summaries/multiturn_2505.20451
type: summary
source_url: https://arxiv.org/abs/2505.20451
source_type: paper
date: 2025-05-26
ingested: 2026-04-11
tags: [LLM Judge, Multi-turn Conversation, Dialog Acts, Grice Maxims, AMULET]
---

# AMULET: Putting Complex Multi-Turn Conversations on the Stand with LLM Juries

**Source**: [arXiv 2505.20451](https://arxiv.org/abs/2505.20451) · 2025-05-26

## Key takeaways

- AMULET: 基于对话行为和 Grice 准则的 LLM 评判框架
- 人类 60-70% 概率在连续轮次中改变意图
- ~75% 的 preference response 可通过 dialog acts 和/或 maxims 区分
- 集成单个 judge 或 multiple LLM juries

## Core claims

AMULET 针对当前 LLM 评判器在复杂多轮对话评估上的不足，提出基于语言学理论的改进框架。核心观察：(1) Dialog Acts - 分析每轮对话的交际结构（ communicative dimensions 和 functions），揭示人类意图的动态变化；(2) Grice's Maxims - 使用 12 个子准则（包括 Quantity、Quality、Manner、Relation、Benevolence、Transparency）评估回答。实验表明 ~78% 的 preference 实例可通过 DA 和/或 MAXIM 区分。AMULET-LM-JURY 和 AMULET-RM-JURY 在四个数据集上显著超越基线。

## Concepts introduced / referenced

- [[LLM Judge]]
- [[Dialog Acts]]
- [[Grice Maxims]]
- [[AMULET]]
- [[LLM Jury]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2505.20451_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告