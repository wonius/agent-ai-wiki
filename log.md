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

## [2026-04-08] compile | skillrouter 补充：新增3个 concept pages（技能路由、Retrieve-and-Rerank、Progressive Disclosure）+ 文件规范化

## [2026-04-08] compile | ACON 规范化：重命名 PDF（acon.pdf → ACON_2510.00615.pdf）、更正 arxiv URL、补充3个 concept pages（压缩准则优化、对比轨迹、语义压缩）+ 2个 entity pages（Microsoft Research、KAIST）+ 更新 index/log
