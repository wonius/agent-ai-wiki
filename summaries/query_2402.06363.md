---
title: summaries/query_2402.06363
type: summary
source_url: https://arxiv.org/abs/2402.06363
source_type: paper
date: 2024-02-09
ingested: 2026-04-11
tags: [LLM Security, Prompt Injection, Structured Queries]
---

# StruQ: Defending Against Prompt Injection with Structured Queries

**Source**: [arXiv 2402.06363](https://arxiv.org/abs/2402.06363) · USENIX Security Symposium 2025

## Key takeaways

- Introduces structured queries separating prompts and data into two channels
- Novel fine-tuning strategy converts base LLM to structured instruction-tuned model
- Reduces manual prompt injection attack success rate to <2% on Llama/Mistral
- Decreases TAP attack success rate from 97% to 9%, GCG from 97% to 58%
- Minimal or no impact on utility (AlpacaEval)

## Core claims

Prompt injection is the #1 security risk for LLM applications. StruQ proposes a safe-by-design API separating control (prompt) from data, trained with structured instruction tuning that teaches LLMs to only follow instructions in the prompt portion, not in the data portion.

## Notable quotes

> Prompt injection has been dubbed the #1 security risk for LLM applications by OWASP.

## Concepts introduced / referenced

- [[提示注入]]
- [[结构化查询]]
- [[安全设计]]
- [[指令微调]]

## Sources

- [[raw/papers/query_2402.06363_深度阅读报告.md]]