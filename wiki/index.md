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
- [[小模型策略]] — 跨论文主题：AgentFold/SkillRouter/ConvSearch-R1 证明小模型+专项策略可超越 10-20x 大模型，核心是找准关键信号+专项优化
- [[检索增强生成]] — RAG 技术：通过外部知识库检索增强 LLM 生成能力
- [[Query 重写]] — 会话搜索 Query 重写：将含指代/省略/歧义的查询转为可检索的独立查询

## Entities

- [[Microsoft Research]] — ACON 论文主要作者团队（Wei-Ning Chen, Dongge Han 等），GitHub: microsoft/acon
- [[KAIST]] — Minki Kang（ACON 第一作者）所属院校，韩国顶尖研究型大学
- [[阿里巴巴通义实验室]] — AgentFold 和 SkillRouter 论文作者团队，Tongyi Lab，Alibaba Group
- [[复旦大学]] — ConvSearch-R1 作者团队，计算机科学技术学院

## Summaries

- [[summaries/acon]] — ACON 论文完整摘要：压缩准则优化、蒸馏 pipeline、26-54% 内存降低
- [[summaries/convsearch-r1]] — ConvSearch-R1 论文完整摘要：检索信号驱动RL、Rank-Incentive Reward、RIRS
- [[summaries/agentfold]] — AgentFold 论文完整摘要：核心问题、多尺度状态摘要、两种折叠操作、关键结果
- [[summaries/skillrouter]] — SkillRouter 论文完整摘要：1.2B 全文检索+重排、80K 技能路由、74.0% Hit@1、5.8× 加速

## New Summaries (2026-04-11)

### Plan (8)
- [[summaries/plan_2401.07324]] — Small LLMs Are Weak Tool Learners: α-UMi multi-LLM framework
- [[summaries/plan_2402.02716]] — Understanding the Planning of LLM Agents: taxonomy & survey
- [[summaries/plan_2408.00764]] — AgentGen: automatic environment & task generation
- [[summaries/plan_2410.02958]] — AutoML-Agent: full AutoML pipeline via multi-agent
- [[summaries/plan_2412.20505]] — CUP: cyclical urban planning with living-judging loop
- [[summaries/plan_2503.17309]] — LLM+MAP: dual-armed robot PDDL planning
- [[summaries/plan_2508.08322]] — Context Engineering for Code Assistants: 4-component workflow
- [[summaries/plan_2512.21309]] — AgentReuse: plan reuse mechanism, 93.12% latency reduction

### Context (8)
- [[summaries/context_2311.03498]] — ICL as Associative Memory: Hopfield network theory
- [[summaries/context_2407.09450]] — EM-LLM: episodic memory, 10M token retrieval
- [[summaries/context_2506.06485]] — Task-Dependent Context-Memory Conflict analysis
- [[summaries/context_2506.08184]] — Proactive Interference: working memory bottleneck
- [[summaries/context_2509.18868]] — LLM Memory Taxonomy & Governance framework
- [[summaries/context_2602.02108]] — OOMB: single-GPU 4M token training
- [[summaries/context_2603.04814]] — (metadata incomplete)
- [[summaries/context_2603.09023]] — Pichay: demand paging, 93% context reduction

### Tool (8)
- [[summaries/tool_2308.13062]] — ZeroLeak: LLM patches side-channel vulnerabilities
- [[summaries/tool_2401.07324]] — Multi-LLM Agent: planner/caller/summarizer decomposition
- [[summaries/tool_2405.07551]] — MuMath-Code: math + code tools fusion, 90.7% GSM8K
- [[summaries/tool_2406.01698]] — GenZ: LLM reasoning platform analysis
- [[summaries/tool_2406.12276]] — CodeNav: code repository navigation
- [[summaries/tool_2411.16313]] — CATP-LLM: cost-aware tool planning
- [[summaries/tool_2503.01763]] — ToolRet: tool retrieval benchmark (7.6k tasks, 43k tools)
- [[summaries/tool_2510.11536]] — CodeWatcher: IDE telemetry data collection

### Query (8)
- [[summaries/query_2308.15272]] — AutoDroid: LLM-powered Android automation
- [[summaries/query_2401.07324]] — (duplicate, see plan)
- [[summaries/query_2402.06363]] — StruQ: prompt injection defense
- [[summaries/query_2405.19749]] — LLM-based query recommendation
- [[summaries/query_2501.01711]] — Legal aid query understanding
- [[summaries/query_2504.06356]] — Query understanding in LLM-based CIS
- [[summaries/query_2506.21384]] — Omni-RAG: live RAG
- [[summaries/query_2509.09690]] — Job matching query enhancement

### Multi-turn (8)
- [[summaries/multiturn_2311.17376]] — CESAR: compositional instruction induction
- [[summaries/multiturn_2402.13146]] — OLViT: multi-modal state tracking
- [[summaries/multiturn_2408.01725]] — Drama Machine: character development simulation
- [[summaries/multiturn_2409.11500]] — Multi-document grounded dialog generation
- [[summaries/multiturn_2411.12307]] — Intent classification accuracy vs efficiency
- [[summaries/multiturn_2502.13298]] — RealTOD: prompt chaining + fine-grained feedback
- [[summaries/multiturn_2505.20451]] — AMULET: LLM jury for complex conversations
- [[summaries/multiturn_2510.08158]] — Scenario-based refusal diagnostics

### Anaphora (8)
- [[summaries/anaphora_2308.14870]] — R-matrix for plasma opacities
- [[summaries/anaphora_2404.14883]] — LLM vs Human language understanding scaling
- [[summaries/anaphora_2412.14501]] — LLM + reasoningism
- [[summaries/anaphora_2502.12509]] — (withdrawn)
- [[summory/anaphora_2502.14119]] — Anaphora Accessibility for discourse
- [[summaries/anaphora_2506.18091]] — Czech coreference resolution
- [[summaries/anaphora_2509.11466]] — Improved LLM coreference
- [[summaries/anaphora_2509.17505]] — CorefInst multilingual coreference

## New Concepts (2026-04-11)

### Planning
- [[Plan Decomposition]] — α-UMi-style multi-LLM role splitting (planner/caller/summarizer)
- [[Automatic Task Generation]] — AgentGen approach to synthetic environments/tasks
- [[Plan Reuse]] — AgentReuse: caching and reusing agent plans for latency reduction
- [[Context Engineering]] — Systematic 4-component workflow for code assistant contexts

### Context & Memory
- [[Episodic Memory]] — EM-LLM: human-inspired情景记忆 for 10M token retrieval
- [[Context-Memory Conflict]] — Task-dependent balance between live context and stored memory
- [[Proactive Interference]] — Working memory bottleneck in LLM context management
- [[Memory Governance]] — Unified framework for LLM memory lifecycle (mechanism/evaluation/governance)
- [[Demand Paging]] — Pichay: OS-style on-demand context loading, 93% reduction

### Tool Use
- [[Tool Decomposition]] — Splitting tool use into planner/caller/summarizer components
- [[Tool Retrieval]] — ToolRet benchmark: 7.6k tasks, 43k tools for retrieval evaluation
- [[Cost-Aware Planning]] — CATP-LLM: optimizing tool selection by API cost
- [[Code Repair]] — ZeroLeak: LLM-based automated vulnerability patching

### Query Understanding
- [[Query Rewriting]] — Converting contextual queries to standalone retrievable form
- [[Intent Classification]] — Multi-turn dialogue intent tracking
- [[Live RAG]] — Omni-RAG: real-time retrieval-augmented generation

### Multi-turn Dialogue
- [[Instruction Induction]] — CESAR: automatic compositional instruction generation
- [[Prompt Chaining]] — RealTOD: multi-step task completion via chained prompts
- [[Refusal Diagnostics]] — Beyond over-refusal: scenario-based LLM behavior analysis

### Anaphora & Coreference
- [[LLM Coreference]] — Improving LLM's anaphora resolution capabilities
- [[Multilingual Coreference]] — CorefInst: cross-lingual coreference benchmark
- [[Discourse Understanding]] — Anaphora Accessibility theory for discourse-level LLM

## New Entities (2026-04-11)

- [[中山大学]] — plan_2401.07324 authors (Shen, Li, Quan) from Sun Yat-sen University
- [[阿里巴巴]] — Multiple papers: plan_2402, context_2602, multiturn_2411
- [[微软研究院]] — tool_2308.13062 (ZeroLeak)
- [[Stanford]] — query_2504.06356 (CIS research)
- [[CMU]] — multiturn_2408.01725 (Drama Machine)
