---
title: summaries/anaphora_2502.14119
type: summary
source_url: https://arxiv.org/abs/2502.14119
source_type: paper
date: 2025-02-19
ingested: 2026-04-11
tags: [LLM, Discourse Understanding, Anaphora Accessibility, Dynamic Semantics]
---

# Meaning Beyond Truth Conditions: Evaluating Discourse Level Understanding via Anaphora Accessibility

**Source**: [arXiv 2502.14119](https://arxiv.org/abs/2502.14119) · 2025-02-19

## Key takeaways

- Proposes hierarchy of semantic understanding levels: lexical → sentence → discourse
- Introduces anaphora accessibility as diagnostic for discourse-level NLU in LLMs
- Tests LLMs and humans on quantifier scope interactions with anaphora (existential vs. universal, negation, disjunction, conditionals)
- LLMs and humans align on some tasks, diverge on others
- Divergence explained by LLMs' reliance on lexical items vs. human sensitivity to structural abstractions

## Core claims

The paper argues for moving beyond lexical/sentence-level NLU assessments to discourse-level evaluation. Anaphora accessibility reveals whether LLMs can represent and update discourse states like humans — specifically, whether they understand semantic scope boundaries that determine referent accessibility.

## Key findings

- Universal quantifiers: anaphora infelicitous outside scope (e.g., "Every farmer worked in his field. He dreamed..." → #)
- Existential quantifiers: anaphora accessible (e.g., "A farmer worked in his field. He dreamed..." → ✓)
- Donkey conditionals: similar scope restrictions apply
- LLMs show human-like sensitivity in some cases but not systematic

## Concepts introduced / references

- [[Anaphora accessibility]]
- [[动态语义学]]
- [[ Discourse state]]
- [[Quantifier scope]]
- [[Donkey anaphora]]

## Sources

- [[wiki/raw/papers/anaphora_2502.14119_深度阅读报告.md]]