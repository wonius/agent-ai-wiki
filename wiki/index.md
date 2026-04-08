# Index — Agent AI 研究

Master catalog of all wiki pages. Updated on every ingest.

## Concepts

- [[上下文管理]] — LLM Agent 上下文的策略与技术，External vs Intra-Task 两条路线
- [[ReAct 范式]] — 当前 Web Agent 主流范式，"推理→行动→观察"循环，致命缺陷是上下文饱和
- [[上下文饱和]] — ReAct append-only 导致的关键信息被噪声淹没问题，附量化分析
- [[多尺度状态摘要]] — AgentFold 核心数据结构，由 s(x,y) 摘要块组成的有序序列
- [[深度整合]] — AgentFold 折叠操作之一，k<t-1 时触发，压缩多步为粗粒度摘要
- [[粒度凝缩]] — AgentFold 折叠操作之一，k=t-1 时触发，保护单步关键细节不被后续压缩影响
- [[技能路由]] — SkillRouter 研究的核心问题：在超大规模技能库中如何精准定位相关技能
- [[Retrieve-and-Rerank]] — 两阶段排序范式：稀疏检索召回 Top-K + Reranker 重排，精度与效率平衡
- [[Progressive Disclosure]] — LLM Agent 技能系统的设计策略：隐藏技能体以减少上下文，代价是 31-44 pp 精度损失
- [[压缩准则优化]] — ACON 核心方法：压缩准则 Prompt 本身是可优化资产，自然语言空间梯度，无需更新模型参数
- [[对比轨迹]] — ACON 的对比学习信号来源：完整上下文成功 vs 压缩上下文失败的轨迹对比，提供密集反馈
- [[语义压缩]] — ACON 的压缩层次：语义级保留异构上下文信息（因果/状态/前置条件），与 KV Cache 压缩正交

## Entities

- [[Microsoft Research]] — ACON 论文主要作者团队（Wei-Ning Chen, Dongge Han 等），GitHub: microsoft/acon
- [[KAIST]] — Minki Kang（ACON 第一作者）所属院校，韩国顶尖研究型大学
- [[阿里巴巴通义实验室]] — AgentFold 和 SkillRouter 论文作者团队，Tongyi Lab，Alibaba Group

## Summaries

- [[summaries/acon]] — ACON 论文完整摘要：压缩准则优化、蒸馏 pipeline、26-54% 内存降低
- [[summaries/convsearch-r1]] — ConvSearch-R1 论文完整摘要：检索信号驱动RL、Rank-Incentive Reward、RIRS
- [[summaries/agentfold]] — AgentFold 论文完整摘要：核心问题、多尺度状态摘要、两种折叠操作、关键结果
- [[summaries/skillrouter]] — SkillRouter 论文完整摘要：1.2B 全文检索+重排、80K 技能路由、74.0% Hit@1、5.8× 加速
