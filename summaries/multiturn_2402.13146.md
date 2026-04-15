---
title: summaries/multiturn_2402.13146
type: summary
source_url: https://arxiv.org/abs/2402.13146
source_type: paper
date: 2024-02-20
ingested: 2026-04-11
tags: [Video Dialog, Multi-Modal, Dialog State Tracking, OLViT]
---

# OLViT: Multi-Modal State Tracking via Attention-Based Embeddings for Video-Grounded Dialog

**Source**: [arXiv 2402.13146](https://arxiv.org/abs/2402.13146) · 2024-02-20

## Key takeaways

- OLViT (Object Language Video Transformer)：视频对话的多模态状态追踪框架
- 提出 Object State Tracker (OST) 和 Language State Tracker (LST) 两个注意力机制组件
- DVD 数据集达到 54.85% 准确率（+3.75% SOTA），SIMMC 2.1 BLEU-4 达到 28.30/25.20
- 通用框架，可无缝集成到预训练 LLM 中

## Core claims

OLViT 针对视频对话中的三个核心挑战提出解决方案：(1) 空间和时间定位问题 - 通过 OST 追踪最相关的视觉对象；(2) 长时序推理问题 - 通过 LST 追踪跨对话轮次的语言共指关系；(3) 多轮对话中的对象追踪问题。两大状态追踪器在每一轮对话后更新全局对话状态，学习连续的多模态状态表示，可灵活集成到各类 LLM 架构中。

## Concepts introduced / referenced

- [[视频对话]]
- [[多模态学习]]
- [[对话状态追踪]]
- [[OLViT]]
- [[Object State Tracker]]
- [[Language State Tracker]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2402.13146_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告