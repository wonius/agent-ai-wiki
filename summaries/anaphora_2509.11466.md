---
title: summaries/anaphora_2509.11466
type: summary
source_url: https://arxiv.org/abs/2509.11466
source_type: paper
date: 2025-09-14
ingested: 2026-04-11
tags: [Coreference Resolution, LLM, Hallucination, Training Techniques]
---

# Improving LLMs' Learning for Coreference Resolution

**Source**: [arXiv 2509.11466](https://arxiv.org/abs/2509.11466) · 2025-09-14

## Key takeaways

- Investigates limitations of existing LLM-based coreference resolution approaches
- Two main methods examined: QA Template and Document Template
- Proposes two novel techniques:
  1. **Reversed Training with Joint Inference** — improves QA Template method
  2. **Iterative Document Generation** — eliminates hallucinations in source text
- Both techniques combined provide effective and robust CR solution

## Core claims

Existing LLM approaches to coreference resolution suffer from hallucination issues (Document Template) or underperformance (QA Template). The proposed techniques address both problems:

- Reversed Training leverages bidirectional coreferential relations
- Iterative Document Generation prevents hallucination entirely while improving CoNLL scores

## Key innovations

1. **Reversed Training**: Coreference relations are bidirectional — training on reversed pairs increases useful information
2. **Iterative Document Generation**: Avoids hallucinations by generating documents incrementally rather than all-at-once

## Concepts introduced / references

- [[Coreference resolution]]
- [[QA Template]]
- [[Document Template]]
- [[LLM hallucination]]
- [[Reversed training]]

## Sources

- [[wiki/raw/papers/anaphora_2509.11466_深度阅读报告.md]]