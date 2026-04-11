---
title: summaries/context_2509.18868
type: summary
source_url: https://arxiv.org/abs/2509.18868
source_type: paper
date: 2025-09-23
ingested: 2026-04-11
tags: [LLM Memory, Taxonomy, Evaluation, Governance, Parametric, Contextual, External, Procedural]
---

# Memory in Large Language Models: Mechanisms, Evaluation and Evolution

**Source**: [arXiv 2509.18868](https://arxiv.org/abs/2509.18868) · 2025-09-23

## Key takeaways

- Unified operational definition: memory = persistent and addressable state
- Four-part taxonomy: parametric, contextual, external, procedural/episodic
- Memory quadruple: location, persistence, write/access path, controllability
- Three-setting protocol decouples capability from information availability
- Layered evaluation framework across four memory types
- DMM Gov: dynamic governance for updating and forgetting

## Core claims

This paper builds an "operating system" for LLM memory governance. Defines LLM memory as persistent state written during pretraining, finetuning, or inference that can later be addressed and stably influences outputs. Proposes four-way taxonomy and memory quadruple. Connects mechanism, evaluation, and governance through causal chain: write → read → inhibit/update. Addresses three core challenges: blurred conceptual boundaries, fragmented evaluation lenses, and bias in automated judging.

## Notable quotes

> "When a physician consults an AI assistant for the latest treatment options but receives recommendations for a drug that was phased out three years ago"

## Concepts introduced / referenced

- [[参数化记忆]]
- [[情境记忆]]
- [[外部记忆]]
- [[程序性/情景记忆]]
- [[DMM-Gov]]
- [[记忆四元组]]
- [[RAG]]

## Sources

- [[wiki/raw/papers/context_2509.18868_extracted.json]] — 2026-04-11 提取数据