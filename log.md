# Log

Append-only chronological record of all wiki operations.
Format: `## [YYYY-MM-DD] <op> | <description>`
Ops: `ingest`, `compile`, `query`, `lint`, `promote`, `split`

Quick grep: `grep "^## \[" log.md | tail -10`

---

## [2026-04-07] scaffold | Initialized Agent AI 研究 wiki knowledge base

## [2026-04-07] ingest | agentfold — AgentFold 论文（arXiv 2510.24699）：长程 Web Agent 主动上下文折叠范式，30B 超越 671B

## [2026-04-07] compile | AgentFold 论文摘要（1篇 summary）+ 3个 concept pages（上下文管理、ReAct 范式、上下文饱和）+ 1个 entity page（阿里巴巴通义实验室）

## [2026-04-08] ingest | skillrouter — SkillRouter 论文（arXiv 2603.22455）：1.2B Retrieve-and-Rerank 管道，80K 技能路由，74.0% Hit@1，5.8× 加速，含原始 PDF + 深度阅读报告

## [2026-04-08] cleanup | 移除3篇误 ingest 的无关论文（horizonweaver、diffusion-restoration、email-deception）

## [2026-04-09] ingest | skillrouter — SkillRouter 论文深度阅读（arXiv 2603.22455）：全10章深度阅读报告 + wiki summary 更新 + 矛盾检测 + 知识缺口追加 reverse-prompts

## [2026-04-09] ingest | acon — ACON 论文（arXiv 2510.00615）：Agent Context Optimization，压缩准则优化 + 蒸馏，26-54% peak tokens 降低，小模型 Agent 精度 +20-46%

## [2026-04-08] compile | skillrouter 补充：新增3个 concept pages（技能路由、Retrieve-and-Rerank、Progressive Disclosure）+ 文件规范化

## [2026-04-08] compile | ACON 规范化：重命名 PDF（acon.pdf → ACON_2510.00615.pdf）、更正 arxiv URL、补充3个 concept pages（压缩准则优化、对比轨迹、语义压缩）+ 2个 entity pages（Microsoft Research、KAIST）+ 更新 index/log

## [2026-04-08] compile | ConvSearch-R1 + AgentFold + SkillRouter 规范化
- ConvSearch-R1：重命名 PDF（convsearch-r1.pdf → ConvSearch-R1_2503.21278.pdf），新增 concept pages（检索增强生成、Query 重写）+ entity page（复旦大学），修复悬垂链接 [[SkillBench]]
- AgentFold：重命名 PDF（agentfold_2510.24699.pdf → AgentFold_2510.24699.pdf），更新 sources 路径，补充通义实验室 entity（加入 SkillRouter 作者信息）
- SkillRouter：修复悬垂链接 [[SkillBench]] → [[AgentFold]]
- Index 重写（结构修复），补全所有 concept + entity 条目

## [2026-04-09] contradict | 语义矛盾检测：4篇 summary（ACON/AgentFold/SkillRouter/ConvSearch-R1）两两对比
- 结论：无直接矛盾
- 发现：ACON（语义压缩）和 AgentFold（主动折叠）解决同一问题（上下文饱和），角度互补
- 更新：上下文饱和.md 新增与 ACON 对比表；上下文管理.md 新增 ACON 条目（Intra-Task 路线）

## [2026-04-09] promote | 小模型策略 — 综合 AgentFold/SkillRouter/ConvSearch-R1 三个案例，跨论文主题浮现

## [2026-04-09] ingest | convsearch-r1
