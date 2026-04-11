---
title: summaries/multiturn_2311.17376
type: summary
source_url: https://arxiv.org/abs/2311.17376
source_type: paper
date: 2023-11-29
ingested: 2026-04-11
tags: [Multi-turn Dialog, Instruction Tuning, Compositional Tasks, CESAR]
---

# CESAR: Automatic Induction of Compositional Instructions for Multi-turn Dialogs

**Source**: [arXiv 2311.17376](https://arxiv.org/abs/2311.17376) · 2023-11-29

## Key takeaways

- CESAR: 自动组合对话任务的指令框架，无需人工干预
- 发布 InstructDial++ benchmark：63 datasets, 86 basic tasks, 68 composite tasks
- 证明 compositional 任务数据可提升 both atomic 和 compositional 任务性能
- 弥补开源模型与 ChatGPT 在复杂指令跟随上的差距

## Core claims

CESAR 提出了一种无监督的指令组合方法，将多个原子对话任务的 prompt 部分自动组合生成复合任务指令。通过分析公开模型与闭源模型（如 ChatGPT）在复杂约束对话任务上的表现差距，得出大规模复合训练数据是提升模型指令跟随能力的关键结论。实验表明在训练数据中加入 compositional 任务不仅提升 composite 任务表现，也能改善 atomic 任务的性能。

## Concepts introduced / referenced

- [[指令微调]]
- [[InstructDial++]]
- [[CESAR]]
- [[Compositional Instructions]]
- [[对话状态追踪]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2311.17376_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告