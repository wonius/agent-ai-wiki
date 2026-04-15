---
title: summaries/anaphora_2509.17505
type: summary
source_url: https://arxiv.org/abs/2509.17505
source_type: paper
date: 2025-09-22
ingested: 2026-04-11
tags: [Multilingual Coreference, LLMs, Zero Mentions, Pro-drop Languages]
---

# CorefInst: Leveraging LLMs for Multilingual Coreference Resolution

**Source**: [arXiv 2509.17505](https://arxiv.org/abs/2509.17505) · 2025-09-22

## Key takeaways

- First multilingual CR methodology using decoder-only LLMs for both overt and zero mentions
- Explores five different instruction sets with controlled inference method
- Evaluated on Llama 3.1, Gemma 2, Mistral 0.3
- Best model: fully fine-tuned Llama 3.1 outperforms Corpipe 24 by 2pp average across all languages in CorefUD v1.2
- Handles zero mentions in pro-drop languages (Chinese, Turkish, Hungarian, Czech)

## Core claims

Instruction-tuned LLMs can surpass state-of-the-art task-specific architectures for multilingual coreference resolution when using suitable instruction sets. The approach handles both overt mentions and zero mentions (dropped pronouns in pro-drop languages).

## Key innovations

1. **Instruction engineering**: Five different instruction sets tested to model CR task for LLMs
2. **Controlled inference method**: Recursive search with feedback to refine formats
3. **Zero mention handling**: Addresses pro-dropped languages where pronouns are omitted

## Results

- Fine-tuned Llama 3.1 beats Corpipe 24 single-stage by ~2pp across CorefUD languages
- Superior performance on pro-drop languages specifically

## Concepts introduced / references

- [[Multilingual coreference resolution]]
- [[Zero mentions]]
- [[Pro-drop languages]]
- [[CorefUD]]
- [[Corpipe]]

## Sources

- [[wiki/raw/papers/anaphora_2509.17505_深度阅读报告.md]]