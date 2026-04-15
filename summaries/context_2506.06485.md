---
title: summaries/context_2506.06485
type: summary
source_url: https://arxiv.org/abs/2506.06485
source_type: paper
date: 2025-06-09
ingested: 2026-04-11
tags: [LLM, Context-Memory Conflict, Knowledge Requirements, Task Dependency]
---

# Task Matters: Knowledge Requirements Shape LLM Responses to Context–Memory Conflict

**Source**: [arXiv 2506.06485](https://arxiv.org/abs/2506.06485) · 2025-06-09

## Key takeaways

- Context–memory conflict is inherently task-dependent
- Performance degradation driven by both task-specific knowledge reliance and conflict plausibility
- Strategies like rationales or context reiteration increase context reliance
- Helps context-only tasks but harms those requiring parametric knowledge
- Findings bias model-based evaluation (LLM-as-a-Judge)

## Core claims

LLMs draw on both contextual information and parametric memory, yet these sources can conflict. Prior studies examined this in contextual QA, implicitly assuming tasks should rely on provided context. This work introduces a model-agnostic diagnostic framework varying task formulations while holding underlying knowledge constant. Shows that impact of context–memory conflict correlates with task's knowledge requirements, not just conflict level.

## Concepts introduced / referenced

- [[上下文-记忆冲突]]
- [[参数化知识]]
- [[情境知识]]
- [[知识冲突]]
- [[模型评估]]

## Sources

- [[wiki/raw/papers/context_2506.06485_extracted.json]] — 2026-04-11 提取数据