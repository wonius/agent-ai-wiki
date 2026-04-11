# 深度阅读报告：arXiv:2505.20451v1 [cs.CL] 26 May 2025 AMULET: Putting Complex Multi-Turn Conversations on the Stand with LLM Juries Sahana Ramnath, Anurag Mudgil, Brihi Joshi, Skyler Hallinan, Xiang Ren Department of Computer Science, University of Southern California sramnath@usc.edu Abstract Today, large language models are widely used as judges to evaluate responses from other language models. Hence, it is imperative to benchmark and improve these LLM-judges on realworld language model usage: a typical humanassistant conversation is lengthy, and shows significant diversity in topics, intents, and requirements across turns, e.g. social interactions, task requests, feedback. We present AMULET, a framework that leverages pertinent linguistic concepts of dialog-acts and maxims to improve the accuracy of LLM-judges on preference data with complex, multi-turn conversational context. AMULET presents valuable insights about (a) the communicative structures and intents present in the conversation (dialog acts), and (b) the satisfaction of conversational principles (maxims) by the preference responses, and uses them to make judgments. On four challenging datasets, AMULET shows that (a) humans frequently (60-70% of the time) change their intents from one turn of the conversation to the next, and (b) in ∼75% of instances, the preference responses can be differentiated via dialog acts and/or maxims, reiterating the latter’s significance in judging such data. AMULET can be used either as a judge by applying the framework to a single LLM, or integrated into a jury with different LLM judges; our judges and juries show strong improvements on relevant baselines for all four datasets (code, data here). 1 Introduction In contemporary NLP literature, it has become common to prompt large language models such as GPT4 (Achiam et al., 2023) as open-ended judges, to evaluate language models (Zheng et al., 2023; Lin et al.) and to generate AI feedback to post-train them (Bai et al., 2022b; Cui et al., 2023; Lee et al.). This has made it imperative to benchmark and improve these LLM-judges on real-life usage of language model assistants by humans. Prior works (Zhao et al., 2024; Laban et al., 2025) show that Hello! I need advice. What can I do with leftover coffee grounds? Hey there! The best way to use them is to compost them, or … Composting kitchen waste is often overlooked by gardeners. Hmm. What benefit is there to putting them in the garden? Okay! What other kitchen garbage is typically composted? . . Eggshells, rinds, tea leaves, .. When composting, be sure to .. ✅ ❌ Social (Greeting) & Task (Request, Question) Feedback (Negative) & Task (Question) Feedback (Positive) & Task (Question) 󰟧 󰟧 󰟧 More relevant response DA MAXIM AMULET judges and juries use communicative structures + conversational principles to judge which response is better. Which response is better for this multi-turn conversation? Figure 1: Real-world language model usage typically includes lengthy, complex human-assistant conversations, where humans express varying intents and requirements across the turns. Given preference data with such context, how accurate are LLM-judges in predicting which response is better? We develop a framework, AMULET, that uses the following linguistic concepts for the same: (a) dialog-acts (DA) to analyze the communicative structure of each turn in the conversation, and (b) maxims to compare the preference responses in terms of principles such as informativity, truth, relevance, etc. human-assistant conversations are often multi-turn, diverse, and complex in nature. However, popular LLM judge evaluation benchmarks (Lambert et al., 2025; Zheng et al., 2023; Li et al., 2023) face the following limitations: conversations in the benchmark are limited to one or two human turns, and conversations are mostly focused on individual, specific tasks (e.g., math reasoning, coding/debugging, logical QA, etc.) without changing requirements and intents. In this work, we take the first step towards analyzing and improving LLMjudges on preference datasets with complex, multiturn conversational context (refer Figure 1). 1

**生成时间**: 2026-04-11 11:20:11  
**源文件**: multiturn_2505.20451.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | arXiv:2505.20451v1 [cs.CL] 26 May 2025 AMULET: Putting Complex Multi-Turn Conversations on the Stand with LLM Juries Sahana Ramnath, Anurag Mudgil, Brihi Joshi, Skyler Hallinan, Xiang Ren Department of Computer Science, University of Southern California sramnath@usc.edu Abstract Today, large language models are widely used as judges to evaluate responses from other language models. Hence, it is imperative to benchmark and improve these LLM-judges on realworld language model usage: a typical humanassistant conversation is lengthy, and shows significant diversity in topics, intents, and requirements across turns, e.g. social interactions, task requests, feedback. We present AMULET, a framework that leverages pertinent linguistic concepts of dialog-acts and maxims to improve the accuracy of LLM-judges on preference data with complex, multi-turn conversational context. AMULET presents valuable insights about (a) the communicative structures and intents present in the conversation (dialog acts), and (b) the satisfaction of conversational principles (maxims) by the preference responses, and uses them to make judgments. On four challenging datasets, AMULET shows that (a) humans frequently (60-70% of the time) change their intents from one turn of the conversation to the next, and (b) in ∼75% of instances, the preference responses can be differentiated via dialog acts and/or maxims, reiterating the latter’s significance in judging such data. AMULET can be used either as a judge by applying the framework to a single LLM, or integrated into a jury with different LLM judges; our judges and juries show strong improvements on relevant baselines for all four datasets (code, data here). 1 Introduction In contemporary NLP literature, it has become common to prompt large language models such as GPT4 (Achiam et al., 2023) as open-ended judges, to evaluate language models (Zheng et al., 2023; Lin et al.) and to generate AI feedback to post-train them (Bai et al., 2022b; Cui et al., 2023; Lee et al.). This has made it imperative to benchmark and improve these LLM-judges on real-life usage of language model assistants by humans. Prior works (Zhao et al., 2024; Laban et al., 2025) show that Hello! I need advice. What can I do with leftover coffee grounds? Hey there! The best way to use them is to compost them, or … Composting kitchen waste is often overlooked by gardeners. Hmm. What benefit is there to putting them in the garden? Okay! What other kitchen garbage is typically composted? . . Eggshells, rinds, tea leaves, .. When composting, be sure to .. ✅ ❌ Social (Greeting) & Task (Request, Question) Feedback (Negative) & Task (Question) Feedback (Positive) & Task (Question) 󰟧 󰟧 󰟧 More relevant response DA MAXIM AMULET judges and juries use communicative structures + conversational principles to judge which response is better. Which response is better for this multi-turn conversation? Figure 1: Real-world language model usage typically includes lengthy, complex human-assistant conversations, where humans express varying intents and requirements across the turns. Given preference data with such context, how accurate are LLM-judges in predicting which response is better? We develop a framework, AMULET, that uses the following linguistic concepts for the same: (a) dialog-acts (DA) to analyze the communicative structure of each turn in the conversation, and (b) maxims to compare the preference responses in terms of principles such as informativity, truth, relevance, etc. human-assistant conversations are often multi-turn, diverse, and complex in nature. However, popular LLM judge evaluation benchmarks (Lambert et al., 2025; Zheng et al., 2023; Li et al., 2023) face the following limitations: conversations in the benchmark are limited to one or two human turns, and conversations are mostly focused on individual, specific tasks (e.g., math reasoning, coding/debugging, logical QA, etc.) without changing requirements and intents. In this work, we take the first step towards analyzing and improving LLMjudges on preference datasets with complex, multiturn conversational context (refer Figure 1). 1 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 26 May 2025 |
| **发表日期** | 未知 |


**百分比数据:** 80100, 50100, 29738, 7080, 96, 84, 80, 78

**浓度数据:** 7M, 20m, 3m, 6M, 26M, 12m

**温度条件:** 30, 024, 023, 022, 021°C

---

## 第二部分：核心理解

*本部分由AI进行深度分析*

### 1. 这篇论文到底在做什么？
[AI分析中...]

### 2. 为什么要做这个？
[AI分析中...]

### 3. 是怎么做到的？
[AI分析中...]

### 4. 做得怎么样？
[AI分析中...]

### 5. 意味着什么？
[AI分析中...]

---

## 第三部分：批判性分析

*本部分由AI进行深度分析*

### 1. 优点/亮点
[AI分析中...]

### 2. 潜在问题/局限
[AI分析中...]

### 3. 未解决的关键问题
[AI分析中...]

---

## 第四部分：用户研究的关联

*本部分由用户补充*

### 1. 相关度评估
- [ ] 高：直接相关，可借鉴
- [ ] 中：间接相关，有参考价值
- [ ] 低：领域较远，仅作了解

**说明**：[由用户填写]

### 2. 可借鉴之处
- 技术方法：[由用户填写]
- 分析思路：[由用户填写]
- 实验设计：[由用户填写]

### 3. 可能的应用场景
- 研究方向：[由用户填写]
- 实际应用：[由用户填写]

### 4. 补充笔记
[由用户填写]

---

*报告生成时间：2026-04-11 11:20:11*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
