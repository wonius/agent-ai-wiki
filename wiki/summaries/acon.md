---
title: ACON 论文摘要
type: summary
source_url: https://arxiv.org/abs/2510.00615
source_type: paper
date: 2025-10-17
ingested: 2026-04-09
sources: ["raw/papers/ACON_2510.00615.pdf", "https://github.com/microsoft/acon"]
tags: [LLM Agent, 上下文压缩, Long-Horizon, Context Management, Distillation, Prompt Optimization]
---

# ACON: Optimizing Context Compression for Long-Horizon LLM Agents

**Source**: [Minki Kang et al. (KAIST, Microsoft, Cambridge)](https://arxiv.org/abs/2510.00615) · October 2025

## Key takeaways

- ACON (Agent Context Optimization) 提出**梯度-free 的压缩准则优化**：通过"完整上下文成功 vs 压缩上下文失败"的对比轨迹，生成自然语言反馈，直接优化压缩 Prompt，无需更新模型参数
- 在 AppWorld、OfficeBench、8-objective QA 三个基准（均需 15+ 交互步）上，ACON 将 peak tokens 降低 **26–54%** 同时保持任务性能；蒸馏后小模型保留 **>95%** 精度
- ACON 是**小模型 Agent 的 equalizer**：压缩不仅降低成本，还能减少信息噪声，使 Qwen3-14B 在 AppWorld 精度提升 **+26.5%**（26.8% → 33.9%），在 8-objective QA 提升 **+46%**

## Core claims

ACON 解决 LLM Agent 在长时序任务中上下文无限膨胀的根本问题。核心创新是**压缩准则优化（Compression Guideline Optimization）**——不优化模型参数，而是通过 UT（Utility Maximization）+ CO（Compression Maximization）两步迭代优化压缩 Prompt。两步分工：UT 步修复"关键信息丢失导致任务失败"，CO 步在保持足够信息的前提下进一步缩短压缩。ACON 进一步将优化后的压缩器蒸馏到小模型（Qwen3-14B 等），实现压缩零额外 API 成本。

## Notable quotes

> "ACON reduces memory usage by 26–54% (peak tokens) while largely preserving task performance, preserves over 95% of accuracy when distilled into smaller compressors, and enhances smaller LMs as long-horizon agents with up to 46% performance improvement."

> "ACON acts as an equalizer, enabling smaller agents with concise but informative contexts to approach the performance of larger models."

## Concepts introduced / referenced

- [[LLM Agent]]
- [[Context Compression]]
- [[Prompt Optimization]]
- [[Distillation (ML)]]
- [[Long-Horizon Task]]
- [[AppWorld]] (benchmark)
- [[OfficeBench]] (benchmark)
- [[History Compression]]
- [[Observation Compression]]
- [[Utility Maximization]]
- [[Compression Maximization]]
