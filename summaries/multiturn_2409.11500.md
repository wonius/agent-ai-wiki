---
title: summaries/multiturn_2409.11500
type: summary
source_url: https://arxiv.org/abs/2409.11500
source_type: paper
date: 2024-09-17
ingested: 2026-04-11
tags: [Multi-turn Dialog, Synthetic Data, RAG, Document Grounded]
---

# Multi-Document Grounded Multi-Turn Synthetic Dialog Generation

**Source**: [arXiv 2409.11500](https://arxiv.org/abs/2409.11500) · 2024-09-17

## Key takeaways

- 多文档检索增强的多轮对话合成技术
- Chain-of-Thought (CoT) 提示驱动的问题分类生成
- LLM-as-a-Judge 过滤错误答案
- 在 CoQA、MultiDoc2Dial、QuAC、OR-QuAC 四个 benchmark 上超越人类生成数据

## Core claims

该论文提出了一种模拟真实 RAG 部署的多文档多轮对话合成框架。核心技术包括：(1) 基于问题分类学的 CoT 提示控制对话流程；(2) 模拟真实检索器，每轮用户输入后更新检索文档；(3) 使用 LLM-as-a-Judge 逐轮过滤答案错误的对话。人工评估表明合成数据具有多样性、连贯性和正确性。实验证明使用合成数据微调的模型在四个公开 benchmark 上持续优于使用人类生成训练数据的模型。

## Concepts introduced / referenced

- [[合成数据生成]]
- [[RAG]]
- [[多轮对话]]
- [[LLM-as-a-Judge]]
- [[Chain-of-Thought]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2409.11500_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告