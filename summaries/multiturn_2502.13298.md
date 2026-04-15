---
title: summaries/multiturn_2502.13298
type: summary
source_url: https://arxiv.org/abs/2502.13298
source_type: paper
date: 2025-12-26
ingested: 2026-04-11
tags: [Task-Oriented Dialog, Prompt Chaining, Fine-Grained Feedback, RealTOD]
---

# RealTOD: Improving Multi-turn Task Completion via Prompt Chaining and Fine-Grained Feedback

**Source**: [arXiv 2502.13298](https://arxiv.org/abs/2502.13298) · 2025-12-26

## Key takeaways

- RealTOD: 改进 LLM-based TOD 系统的提示链 + 细粒度反馈框架
- Full API Call Accuracy 超越 AutoTOD 37.10%（SGD），超越 SimpleTOD 10.32%（BiTOD）
- 零样本泛化到新领域，无需人工编排对话
- 引入细粒度 API 评估指标

## Core claims

RealTOD 针对任务导向对话系统中 LLM 生成 API 调用时常见的幻觉、错误方法名、缺失参数等问题提出两大核心创新：(1) Prompt Chaining - 自动将源域对话转换为与目标域 schema 对齐的 in-context 示例，实现零样本泛化到新任务；(2) Fine-grained Feedback - 验证每个生成的 API 调用是否符合 schema，识别具体错误并提供针对性纠正提示。引入 Full API Call Accuracy 作为核心评估指标，分解为方法名准确率、参数名准确率、参数值准确率和操作符准确率四个子指标。

## Concepts introduced / referenced

- [[任务导向对话]]
- [[Prompt Chaining]]
- [[Fine-Grained Feedback]]
- [[RealTOD]]
- [[API Call Accuracy]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2502.13298_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告