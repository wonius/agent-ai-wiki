---
title: AgentFold 论文摘要
type: summary
created: 2026-04-07
updated: 2026-04-07
sources: ["raw/papers/AgentFold_深度阅读报告.md"]
tags: [Web Agent, 上下文管理, Long-Horizon, Context Management]
---

# AgentFold 论文摘要

## 一句话概括

AgentFold 通过在每步引入**主动上下文折叠（Proactive Context Folding）**操作，让 Web Agent 在长程任务中兼顾信息完整性与上下文精简，30B 模型超越 671B 竞品。

## 核心问题

LLM Web Agent 在长程任务中面临上下文管理的根本两难：
- **ReAct 模式**：append-only 历史 → 上下文饱和（100步后关键信息存活率仅 36.6%）
- **固定全量摘要**：每步压缩全部历史 → 关键细节不可逆丢失（500步后存活率仅 0.66%）

## 核心解法

将 Agent 上下文设计为**动态认知工作空间**，包含两个核心组件：

1. **多尺度状态摘要（Multi-Scale State Summaries）**：由若干摘要块组成的有序序列，每块可以是单步粒度凝缩 s(x,x) 或跨步深度整合 s(x,y)
2. **最新交互（Latest Interaction）**：上一轮的完整思考-行动-观察，提供原始细节供短期决策

每步生成四元响应：思考(Thinking) + 折叠指令(Folding) + 解释(Explanation) + 行动(Action)

两种折叠操作：
- **粒度凝缩（k=t-1）**：压缩单步，保留关键结论，保护细节不被后续压缩影响
- **深度整合（k<t-1）**：合并多步为一句结论，清除已完成子任务的全部噪声

## 关键结果

| 指标 | 数据 |
|------|------|
| BrowseComp | 36.2%（超越 DeepSeek-V3.1-671B 的 30.0%） |
| BrowseComp-ZH | 47.3% |
| WideSearch | 62.1%（超越所有闭源模型） |
| GAIA | 67.0% |
| 100步后上下文 | 仅约 7k tokens（上限 128k 的 5%） |

## 启示

- **上下文管理本身就是一种能力**，让小模型在特定任务上超越大 20 倍的模型
- Agent 不仅要会执行，还要会**管理自己的信息**
- Acting（制定行动）和 Reflecting（制定折叠）互相强化，形成自我调节循环

## 相关概念

- [[上下文饱和]]
- [[ReAct 范式]]
- [[多尺度状态摘要]]
- [[深度整合]]
- [[粒度凝缩]]
