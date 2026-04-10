---
title: summaries/agentfold
type: summary
source_url: https://arxiv.org/abs/2510.24699
source_type: paper
date: 2025-10-29
ingested: 2026-04-09
tags: [Web Agent, 上下文管理, Long-Horizon, Proactive Context Folding, Qwen, SFT]
---

# AgentFold: Long-Horizon Web Agents with Proactive Context Management

**Source**: Tongyi Lab, Alibaba Group · arXiv:2510.24699 · 2025-10-29

## Key takeaways

- **核心贡献**：提出**主动上下文折叠（Proactive Context Folding）**机制，让 Web Agent 在每步动态决定是保留细节还是抽象掉已完成子任务，从根本上解决 ReAct 范式的上下文饱和问题
- **关键结果**：AgentFold-30B-A3B 在 BrowseComp 达 36.2%，超越 DeepSeek-V3.1-671B（30.0%）和 o4-mini（34.0%）；100步后上下文仅 7k tokens，关键信息存活率 92%
- **技术路线**：每步输出四元响应（Thinking + Folding Directive + Explanation + Action）；通过 Fold-Generator 流水线 + SFT 训练，无需 RL
- **核心洞察**：上下文管理本身就是一种能力——让 30B 模型在特定任务上超越 671B 模型

## Core claims

LLM Web Agent 在长程任务中面临上下文完整性与简洁性的根本两难：ReAct append-only 导致上下文饱和（100步后关键信息存活率仅36.6%），固定全量摘要导致不可逆信息丢失（500步后0.66%）。AgentFold 的解决思路是将人类认知中的**回顾性整合（retrospective consolidation）**机制引入 Agent——上下文是动态认知工作空间，每步通过**粒度凝缩**（保留单步关键结论）或**深度整合**（抽象掉已完成子任务）两种操作主动管理。

## Core architecture

AgentFold 上下文由两部分组成：**Multi-Scale State Summaries**（若干摘要块的有序序列，每块可是单步粒度或跨步深度整合）和 **Latest Interaction**（上一轮完整记录）。每步同时输出：Thinking（思考）+ Folding Directive（折叠指令）+ Explanation（解释）+ Tool Call（工具调用）。

## Key results

| Benchmark | AgentFold-30B-A3B | DeepSeek-V3.1-671B | o4-mini |
|:---|:---:|:---:|:---:|
| BrowseComp | **36.2%** | 30.0% | 34.0% |
| BrowseComp-ZH | **47.3%** | — | — |
| WideSearch | **62.1%** | 54.5% | 58.9% |
| GAIA | **67.0%** | — | — |

## Notable quotes

> "AgentFold treats its context as a dynamic cognitive workspace to be actively sculpted, rather than a passive log to be filled." — AgentFold Abstract

> "This performance not only surpasses or matches open-source models of a dramatically larger scale, such as the DeepSeek-V3.1-671B-A37B, but also surpasses leading proprietary agents like OpenAI's o4-mini." — AgentFold Abstract

## Training details

- **基座模型**: Qwen3-30B-A3B（30B 推理塔 + 3B 动作塔）
- **训练方式**: 纯 SFT，无持续预训练，无 RL
- **数据生成**: Fold-Generator 流水线（LLM + 拒绝采样）
- **上下文上限**: 128k tokens；100步后实际使用约 7k tokens

## Open questions

- 折叠决策的内在逻辑难以解释，能否提取为可解释规则？
- 在代码生成、机器人控制等其他模态的泛化性未知
- SFT 学到的策略是否全局最优？RL 能否进一步提升？
- Fold-Generator 生成的轨迹质量对底层 LLM 能力的依赖程度如何？

## Relationship to other concepts

- [[ReAct 范式]] — AgentFold 继承并改造了 ReAct 的推理-行动循环，解决其上下文饱和问题
- [[上下文饱和]] — AgentFold 的核心解决对象，ReAct 在长程任务中的致命缺陷
- [[多尺度状态摘要]] — AgentFold 的核心数据结构，支持粒度凝缩和深度整合两种操作
- [[深度整合]] — AgentFold 两种折叠操作之一，用于抽象已完成子任务

## Sources

- [[raw/papers/AgentFold_2510.24699.pdf]]
- [[raw/papers/AgentFold_2510.24699_深度阅读报告]]
