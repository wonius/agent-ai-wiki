---
title: summaries/anaphora_2506.18091
type: summary
source_url: https://arxiv.org/abs/2506.18091
source_type: paper
date: 2025-06-22
ingested: 2026-04-11
tags: [Anaphora Resolution, Czech, LLM, Fine-tuning, Morphologically Rich Languages]
---

# Evaluating Prompt-Based and Fine-Tuned Approaches to Czech Anaphora Resolution

**Source**: [arXiv 2506.18091](https://arxiv.org/abs/2506.18091) · 2025-06-22

## Key takeaways

- Comparative evaluation of prompt engineering vs. fine-tuning for Czech anaphora resolution
- Dataset derived from Prague Dependency Treebank (56,207 sentences)
- Prompt-based: Mistral Large 2 achieves 74.5% accuracy (three-shot)
- Fine-tuned mT5-large outperforms prompting significantly: 88% accuracy with fewer resources
- Two anaphora types: grammatical (syntax-driven) vs. textual (semantics-required)

## Core claims

For morphologically rich languages like Czech, fine-tuning compact seq2seq models outperforms prompting large LLMs, while requiring less computational resources. Prompt-based approaches are unstable and prone to format errors.

## Key results

| Approach | Best Model | Accuracy |
|----------|-----------|----------|
| Prompting | Mistral Large 2 (3-shot) | 74.5% |
| Fine-tuning | mT5-large | 88% |
| Rule-based baseline | - | 31% |

## Concepts introduced / referenced

- [[Czech anaphora resolution]]
- [[Prague Dependency Treebank]]
- [[mT5]]
- [[Grammatical anaphora]] vs. [[Textual anaphora]]

## Sources

- [[wiki/raw/papers/anaphora_2506.18091_深度阅读报告.md]]