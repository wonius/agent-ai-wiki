---
title: SkillRouter 论文摘要
type: summary
created: 2026-04-08
updated: 2026-04-08
sources: ["papers/2603.22455v1.pdf"]
tags: [Skill Routing, LLM Agent, Reranker, Large-Scale Skills]
---

# SkillRouter 论文摘要

## 一句话概括

SkillRouter 针对大规模技能库（~80K）的技能路由问题，提出基于全文检索+重排的 1.2B 参数管道，在隐藏技能体导致精度下降 31-44 个百分点的情况下，逆势达到 74.0% Hit@1，且比最强基线快 5.8 倍、参数量少 13 倍。

## 核心问题

随着 LLM Agent 的技能生态扩展到数万个可复用技能，每条用户指令都需要从大量候选中精准定位相关技能，但现有 agent 系统存在根本矛盾：

- ** Progressive Disclosure 策略**：只暴露技能名称和描述，隐藏完整实现体 → 路由精度大幅下降（31-44 pp）
- **全量加载**： inference 时暴露所有技能 → 不可行

核心问题：**如何在超大规模技能库中高效准确地完成技能路由？**

## 核心解法

### 关键发现（动机）
在 SkillsBench 基准（约 80K 候选技能）上的实验揭示：隐藏技能体（body）导致路由精度下降 31-44 个百分点，说明**技能体全文是比元数据更关键的路由信号**，而非"小优化"。

### SkillRouter 架构：1.2B Retrieve-and-Rerank 管道

```
Query（用户任务）→ Sparse Retrieval（BM25，初步筛选）→ Reranker（技能体重排，精排）
```

- **稀疏检索阶段**：BM25，从 80K 候选中快速召回 Top-K
- **重排阶段**：1.2B Reranker，将技能体全文作为关键信号，重新排序

### 训练数据构建
在 SkillsBench 上自动构建含 80K 技能的大规模路由训练集。

## 关键结果

| 指标 | 数据 |
|------|------|
| Hit@1（SkillsBench, 80K 候选） | **74.0%**（最强平均 top-1 路由） |
| vs. 隐藏技能体基线 | 精度提升 31-44 pp |
| 参数量 | 1.2B（比最强基线少 13×） |
| 推理速度 | 比最强基线快 5.8× |
| 跨基准泛化 | 在独立构建的补充基准上同样有效 |
| 端到端coding agents研究 | 路由增益可转化为任务成功率提升，能力越强增益越大 |

## 创新点

1. **全文路由信号**：首次系统证明技能体全文是路由的关键信号，progressive disclosure 策略是路由精度损失的主因
2. **Retrieve-and-Rerank 范式**：用 1.2B 小模型 + 两阶段管道解决 80K 规模技能路由，精度超过纯 embedding-based 方法
3. **端到端验证**：从 routing 精度到 coding agent 任务成功的完整链路验证

## 局限性与发展方向

- **评测基准局限**：SkillsBench 来自特定技能来源，泛化性需进一步验证
- **技能体质量依赖**：reranker 效果依赖技能体本身的质量和标准化程度
- **扩展方向**：多模态技能路由、与 agent planning 的更深层联合优化

## 与你研究的关联

SkillRouter 与 Agent 工作流、工具路由等方向高度相关。核心洞察（"全文信号比元数据更关键"）对 agent 的工具选择、记忆召回等设计有直接借鉴意义。
