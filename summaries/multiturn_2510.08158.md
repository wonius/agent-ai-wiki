---
title: summaries/multiturn_2510.08158
type: summary
source_url: https://arxiv.org/abs/2510.08158
source_type: paper
date: 2025-10-09
ingested: 2026-04-11
tags: [LLM Safety, False Refusal, Over-Refusal, Multi-turn, XSB]
---

# Beyond Over-Refusal: Scenario-Based Diagnostics and Post-Hoc Mitigation for Exaggerated Refusals

**Source**: [arXiv 2510.08158](https://arxiv.org/abs/2510.08158) · 2025-10-09

## Key takeaways

- XSB: 单轮拒绝诱导关键词 benchmark
- MS-XSB: 多轮场景化拒绝校准 benchmark
- 揭示夸张拒绝在多轮场景中更严重
- 三种 post-hoc 缓解策略：ignore-word、prompt rephrasing、attention steering
- 无需重训或参数访问的模型无关方案

## Core claims

该论文系统诊断 LLM 的过度安全行为（false refusal）。贡献包括：(1) XSB benchmark - 340 个 safe prompts + 240 个 unsafe prompts，标注"Focus"关键词识别拒绝触发词；(2) MS-XSB benchmark - 30 场景 × 20 prompts = 600 prompts，评估多轮对话中的拒绝校准；(3) Post-hoc mitigation - 使用 SHAP、feature ablation、integrated gradients 识别触发词后，应用三种无需重训的轻量级干预策略。实验表明这些策略显著提升安全 prompts 的遵从率，但仍需权衡安全性。

## Concepts introduced / referenced

- [[LLM 安全]]
- [[False Refusal]]
- [[Over-Refusal]]
- [[XSB]]
- [[MS-XSB]]
- [[Post-hoc Mitigation]]

## Incomplete sections

深度阅读报告中的"核心理解"和"批判性分析"部分尚未填写完整内容（2026-04-11 标记为 [AI分析中...]）。

## Sources

- [[raw/papers/multiturn_2510.08158_深度阅读报告.md]] — 2026-04-11 深度阅读框架报告