---
title: summaries/skillrouter-2603-22455
type: summary
source_url: https://arxiv.org/abs/2603.22455
source_type: paper
date: 2026-03-24
ingested: 2026-04-11
tags: [LLM Agent, Skill Routing, Retrieval]
---

# SkillRouter: Skill Routing for LLM Agents at Scale

**Source**: [arXiv 2603.22455](https://arxiv.org/abs/2603.22455) · 2026-03-24

## Key takeaways

- Retrieve-and-Rerank pipeline for routing user queries to appropriate skills at scale
- 1.2B parameter model with 80K skill vocabulary
- Hit@1 of 74.0%, achieving 5.8× speedup over dense retrieval baselines
- Smaller model (1.2B) outperforms models 13× larger through full skill manifest as routing signal

## Core claims

SkillRouter demonstrates that a small model (1.2B) can outperform much larger models (13x its size) in skill routing tasks by using the complete skill manifest — including skill descriptions, parameters, and usage patterns — as the routing signal rather than just abstract embeddings. The retrieve-and-rerank architecture enables scalable skill discovery across 80K skills.

## Notable quotes

> (No full quotes available — deep analysis pending)

## Concepts introduced / referenced

- [[技能路由]]
- [[Retrieve-and-Rerank]]
- [[SkillRouter]]
- [[小模型策略]]
- [[Progressive Disclosure]] (contradiction: removing body causes 31-44pp accuracy drop)

## Sources

- [[raw/papers/SkillRouter_2603.22455_深度阅读报告.md]] — 2026-04-10 深度阅读框架报告
