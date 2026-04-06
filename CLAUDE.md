# Agent AI 研究知识库

> Schema document — read at the start of every session.
> Update after every major compile, ingest batch, or structural change.

## Scope

What this wiki covers:
- LLM Agent 架构与设计范式（ReAct、AgentFold 等）
- 上下文管理（Context Management）技术与策略
- Web Agent、Long-Horizon Agent 的核心挑战
- Agent 评测基准（BrowseComp、WideSearch、GAIA 等）

What this wiki deliberately excludes:
- 通用 LLM 技术（非 Agent 相关）
- 与 Agent 无关的纯 NLP 任务

## Naming conventions

- **Concept pages** (`wiki/concepts/`): 中文概念名，如"上下文管理.md"
- **Entity pages** (`wiki/entities/`): 机构/人物名，如"阿里巴巴通义实验室.md"
- **Summary pages** (`wiki/summaries/`): paper-slug，如"agentfold.md"

## Current articles

### Summaries
- `summaries/agentfold` — AgentFold 论文摘要（2026-04-07）

### Concepts
- `上下文管理` — LLM Agent 上下文管理概述
- `ReAct 范式` — ReAct 范式及其上下文饱和问题
- `上下文饱和` — 上下文饱和的量化分析与解决方案对比

### Entities
- `阿里巴巴通义实验室` — AgentFold 论文作者团队

## Open research questions

- AgentFold 的折叠策略在实际 Agent 系统中的工程落地
- 多尺度折叠操作与其他 Agent 范式（Planning、Memory）的结合
- RL 训练的折叠策略 vs SFT 模仿的折叠策略，哪种更优
