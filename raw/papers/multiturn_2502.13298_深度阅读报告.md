# 深度阅读报告：Improving Multi-turn Task Completion in Task-Oriented Dialog Systems via Prompt Chaining and Fine-Grained Feedback Moghis Fereidouni1, Md Sajid Ahmed2, Adib Mosharrof1, A. B. Siddique1 1University of Kentucky, 2Independent Researcher {moghis.fereidouni, adib.mosharrof, ab.siddique}@uky.edu, sajid.ahmed1@northsouth.edu Abstract Task-oriented dialog (TOD) systems facilitate users in accomplishing complex, multi-turn tasks through natural language. While instruction-tuned large language models (LLMs) have demonstrated strong performance on a range of single-turn NLP tasks, they often struggle with reliable multi-turn task completion in TOD settings, particularly when generating API calls required to interact with external systems. To address this, we introduce RealTOD, a novel framework that improves LLM-based TOD systems through (1) prompt chaining and (2) fine-grained feedback. Prompt chaining enables zero-shot generalization to new domains by automatically synthesizing a schema-aligned in-context example for the target task. Fine-grained feedback verifies each generated API call against the domain schema, identifies specific errors, and provides targeted correction prompts. To evaluate task completion reliability, we introduce full API Call Accuracy as a robust metric, along with detailed sub-metrics to capture common failure modes. We conduct extensive experiments on the SGD and BiTOD benchmarks using four LLMs. RealTOD improves Full API accuracy, surpassing state-of-the-art AutoTOD by 37.10% on SGD and supervised learning-based baseline SimpleTOD by 10.32% on BiTOD. Human evaluations further confirm that LLMs integrated with RealTOD achieve superior task completion, fluency, and informativeness compared to existing methods. Introduction Task-oriented dialog (TOD) systems enable users to accomplish tasks, such as booking flights, making restaurant reservations, and managing appointments, through multi-turn, natural language interactions (He et al. 2022). To successfully complete tasks, these systems must understand user intent, retrieve task-specific information from both user and external systems, and generate coherent responses to guide the user toward task completion (e.g., successful reservation). However, traditional TOD systems require extensive domainspecific fine-tuning using manually annotated datasets (Xu et al. 2024; Mi, Wang, and Li 2022), making them laborious and expensive to scale across domains. Recent advances in large language models (LLMs) (Radford et al. 2019; Brown et al. 2020; Chung et al. 2024; Shu et al. 2024) have significantly improved performance across a wide range of single-turn natural language processing (NLP) tasks, such as text classification (Sun et al. 2023; Wang, Pang, Copyright © 2026, Association for the Advancement of Artificial Intelligence (www.aaai.org). All rights reserved. Automatic Conversation for Target Task User Simulator User Goal Parser LLM Execution Multi-turn Natural Language Interactions API Fine-grained Feedback Correct API ✓ ✓ Figure 1: Overview of RealTOD: Prompt chaining circumvents the need for human-curated dialog for each task, and fine-grained feedback loop from the API parser significantly improves task completion rates. and Lin 2023; Zhang et al. 2024b), summarization (Pu, Gao, and Wan 2023; Zhang et al. 2024a; Van Veen et al. 2023), and machine translation (Cui et al. 2025; Wang et al. 2023; Gain, Bandyopadhyay, and Ekbal 2025). In addition to these single-step tasks, LLMs have also shown strong capabilities in multi-turn, open-domain dialog settings (Thoppilan et al. 2022; Naveed et al. 2023; Dubey et al. 2024; Achiam et al. 2023), where coherent response generation given dialog context is critical. Motivated by these advances, recent work has explored leveraging off-the-shelf LLMs for zero-shot TOD settings (Xu et al. 2024). These models can generate fluent and contextually appropriate responses, and with careful prompting, can even be controlled to follow task schemas. However, despite their promise, LLMs often struggle with reliable task completion, particularly when generating API calls required to interact with external systems. Common issues include hallucinated API results, incorrect method names, missing required parameters, and invalid slot-value pairs (Shinn et al. 2023; Jain et al. 2024; Song et al. 2025), all of which can lead to execution failures and incomplete tasks. These issues reveal a critical gap between LLMs’ natural language fluency and their ability to execute tasks reliably in multi-turn dialogs, presenting a major barrier to their practical deployment. To address these issues, we propose RealTOD, a novel framework that enhances LLM-based TOD systems through arXiv:2502.13298v2 [cs.CL] 26 Dec 2025

**生成时间**: 2026-04-11 11:21:05  
**源文件**: multiturn_2502.13298.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | Improving Multi-turn Task Completion in Task-Oriented Dialog Systems via Prompt Chaining and Fine-Grained Feedback Moghis Fereidouni1, Md Sajid Ahmed2, Adib Mosharrof1, A. B. Siddique1 1University of Kentucky, 2Independent Researcher {moghis.fereidouni, adib.mosharrof, ab.siddique}@uky.edu, sajid.ahmed1@northsouth.edu Abstract Task-oriented dialog (TOD) systems facilitate users in accomplishing complex, multi-turn tasks through natural language. While instruction-tuned large language models (LLMs) have demonstrated strong performance on a range of single-turn NLP tasks, they often struggle with reliable multi-turn task completion in TOD settings, particularly when generating API calls required to interact with external systems. To address this, we introduce RealTOD, a novel framework that improves LLM-based TOD systems through (1) prompt chaining and (2) fine-grained feedback. Prompt chaining enables zero-shot generalization to new domains by automatically synthesizing a schema-aligned in-context example for the target task. Fine-grained feedback verifies each generated API call against the domain schema, identifies specific errors, and provides targeted correction prompts. To evaluate task completion reliability, we introduce full API Call Accuracy as a robust metric, along with detailed sub-metrics to capture common failure modes. We conduct extensive experiments on the SGD and BiTOD benchmarks using four LLMs. RealTOD improves Full API accuracy, surpassing state-of-the-art AutoTOD by 37.10% on SGD and supervised learning-based baseline SimpleTOD by 10.32% on BiTOD. Human evaluations further confirm that LLMs integrated with RealTOD achieve superior task completion, fluency, and informativeness compared to existing methods. Introduction Task-oriented dialog (TOD) systems enable users to accomplish tasks, such as booking flights, making restaurant reservations, and managing appointments, through multi-turn, natural language interactions (He et al. 2022). To successfully complete tasks, these systems must understand user intent, retrieve task-specific information from both user and external systems, and generate coherent responses to guide the user toward task completion (e.g., successful reservation). However, traditional TOD systems require extensive domainspecific fine-tuning using manually annotated datasets (Xu et al. 2024; Mi, Wang, and Li 2022), making them laborious and expensive to scale across domains. Recent advances in large language models (LLMs) (Radford et al. 2019; Brown et al. 2020; Chung et al. 2024; Shu et al. 2024) have significantly improved performance across a wide range of single-turn natural language processing (NLP) tasks, such as text classification (Sun et al. 2023; Wang, Pang, Copyright © 2026, Association for the Advancement of Artificial Intelligence (www.aaai.org). All rights reserved. Automatic Conversation for Target Task User Simulator User Goal Parser LLM Execution Multi-turn Natural Language Interactions API Fine-grained Feedback Correct API ✓ ✓ Figure 1: Overview of RealTOD: Prompt chaining circumvents the need for human-curated dialog for each task, and fine-grained feedback loop from the API parser significantly improves task completion rates. and Lin 2023; Zhang et al. 2024b), summarization (Pu, Gao, and Wan 2023; Zhang et al. 2024a; Van Veen et al. 2023), and machine translation (Cui et al. 2025; Wang et al. 2023; Gain, Bandyopadhyay, and Ekbal 2025). In addition to these single-step tasks, LLMs have also shown strong capabilities in multi-turn, open-domain dialog settings (Thoppilan et al. 2022; Naveed et al. 2023; Dubey et al. 2024; Achiam et al. 2023), where coherent response generation given dialog context is critical. Motivated by these advances, recent work has explored leveraging off-the-shelf LLMs for zero-shot TOD settings (Xu et al. 2024). These models can generate fluent and contextually appropriate responses, and with careful prompting, can even be controlled to follow task schemas. However, despite their promise, LLMs often struggle with reliable task completion, particularly when generating API calls required to interact with external systems. Common issues include hallucinated API results, incorrect method names, missing required parameters, and invalid slot-value pairs (Shinn et al. 2023; Jain et al. 2024; Song et al. 2025), all of which can lead to execution failures and incomplete tasks. These issues reveal a critical gap between LLMs’ natural language fluency and their ability to execute tasks reliably in multi-turn dialogs, presenting a major barrier to their practical deployment. To address these issues, we propose RealTOD, a novel framework that enhances LLM-based TOD systems through arXiv:2502.13298v2 [cs.CL] 26 Dec 2025 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 26 Dec 2025 |
| **发表日期** | 未知 |


**百分比数据:** 100, 37.10, 31.96, 26, 20.60, 10.32

**浓度数据:** 5m, 250m, 6m, 12m, 10m, 2.5M

**温度条件:** 025, 024, 023, 022, 020°C

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

*报告生成时间：2026-04-11 11:21:05*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
