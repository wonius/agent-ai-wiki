---
title: ACON 论文摘要
type: summary
created: 2026-04-07
updated: 2026-04-07
sources: ["raw/papers/ACON_2510.00615.pdf", "https://arxiv.org/abs/2510.00615"]
tags: [LLM Agent, 上下文压缩, Long-Horizon, Context Management, Distillation]
---

# ACON 论文摘要

## 一句话概括

ACON (Agent Context Optimization) 通过**自然语言空间的压缩准则优化**，将环境观察和交互历史压缩成简洁但信息丰富的摘要，**在减少 26-54% 内存占用的同时保持任务性能**，且可蒸馏到小模型部署。

## 核心问题

LLM Agent 在长时序任务中面临上下文管理的两大挑战：
- **推理成本高**：Transformer 计算成本随上下文长度线性增长，长上下文带来高昂推理费用
- **信息稀释**：过长的上下文会让模型被无关信息干扰，关键信号被噪声淹没

现有压缩方法的局限：
- 对话摘要系统：适用于会话连贯性，但无法支持多步任务中的状态保持
- 文档级压缩：适用于单步问答（上下文用完即丢），不适用于需要保留跨步状态的多步 Agent
- 现有 Agent 压缩方法：过于狭隘（如只压缩网页无障碍树），或依赖粗糙启发式规则

## 核心方法：ACON

### 压缩什么？

**历史压缩 (History Compression)**：当交互历史长度超过阈值 `T_hist` 时，压缩历史记录 `h' = f(h; φ, P_hist)`

**观察压缩 (Observation Compression)**：当单次环境观察超过阈值 `T_obs` 时，压缩观察结果 `o' = f(o, h; φ, P_obs)`

### 核心创新：压缩准则优化 (Compression Guideline Optimization)

不直接优化压缩模型参数，而是把"压缩准则"当作可优化的 Prompt，在**自然语言空间**通过对比学习信号进行优化：

1. **收集对比轨迹**：完整上下文（成功）vs 压缩上下文（失败）
2. **提取失败子集**：筛选"完整成功但压缩失败"的任务
3. **生成自然语言反馈**：LLM 分析压缩前后差异
4. **更新压缩准则 Prompt**：Utility maximization → Compression maximization 两步迭代

**无梯度、无需更新模型参数**，可直接用于 API 类模型。

### 蒸馏：压缩器小型化

将优化后的压缩器蒸馏到小模型，用交叉熵损失训练学生模型。蒸馏后学生模型可独立做压缩决策，无需调用大模型 API。

## 关键结果

| 指标 | 数据 |
|------|------|
| 内存降低（peak tokens） | 26-54% |
| 蒸馏后精度保留 | >95% |
| 小模型性能提升（AppWorld） | +32% |
| 小模型性能提升（OfficeBench） | +20% |
| 小模型性能提升（Multi-objective QA） | +46% |

## 相关信息

- 论文：https://arxiv.org/abs/2510.00615
- GitHub：https://github.com/microsoft/acon
- 作者：Minki Kang (KAIST), Wei-Ning Chen, Dongge Han 等 (Microsoft)
- 发布：2025年10月
