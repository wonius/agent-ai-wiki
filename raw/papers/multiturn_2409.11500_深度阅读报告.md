# 深度阅读报告：Multi-Document Grounded Multi-Turn Synthetic Dialog Generation Young-Suk Lee∗, Chulaka Gunasekara*, Danish Contractor Ramón Fernandez Astudillo, Radu Florian {ysuklee,raduf}@us.ibm.com {Chulaka.Gunasekara,Danish.Contractor,ramon.astudillo}@ibm.com IBM Research AI Abstract We introduce a technique for multi-documentgrounded multi-turn synthetic dialog generation that incorporates three main ideas. First, we control the overall dialog flow using taxonomy-driven user queries that are generated with Chain-of-Thought (CoT) prompting. Second, we support the generation of multidocument-grounded dialogs by mimicking realworld use of retrievers to update the grounding documents after every user-turn in the dialog. Third, we apply LLM-as-a-Judge to filter out queries with incorrect answers. Human evaluation of the synthetic dialog data suggests that the data is diverse, coherent, and includes mostly correct answers. Both human and automatic evaluations of answerable queries indicate that models fine-tuned on synthetic dialogs consistently out-perform those fine-tuned on existing human generated training data across four publicly available multi-turn document grounded benchmark test sets. 1 Introduction As instruction-tuned language models have proven highly effective to generalize to new tasks, (Chung et al., 2022; Wei et al., 2021; Ouyang et al., 2022; Mishra et al., 2022; Wang et al., 2022b), there has been growing interest to acquire synthetic data sets generated from pre-trained language models with a minimal or no human supervision, (Honovich et al., 2022; Wang et al., 2023; Xu et al., 2023; Lee et al., 2023). While there has been an exploration of synthetic data generation for persona-grounded dialog generation (Jang et al., 2022; Bao et al., 2023), topic-grounded dialog generation (Abbasiantaeb et al., 2024), summary-grounded dialog generation (Gunasekara et al., 2021) and conversation generation from dialog scenarios (Mohapatra et al., 2021), the study of methods for generating multidocument grounded multi-turn dialogs by mimicking real-world deployments with retrievers has *Primary authors with equal contributions been limited (Bao et al., 2023; Abbasiantaeb et al., 2024). Generating a realistic multi-turn document grounded dialog can be challenging as one needs to ensure that the (i) the questions are diverse (ii) conversation flows naturally - i.e, it is coherent and the follow-up questions are more than just a collection of question-answer pairs with co-references (iii) the responses are faithful to the documents in the retrieved set and not generated from model parameters. In this paper, we address these challenges and present a technique for generating high quality document grounded multi-turn synthetic dialogs. Our data generation flow incorporates two novel ideas - first, we control the dialog flow according to a question-taxonomy with chain-of-thought (CoT) prompting for query generation. Second, we generate dialogs grounded on both single and multiple retrieved documents. We also apply self-consistency (Wang et al., 2022a) and an LLM-as-judge (Zheng et al., 2023) to ensure the answers are correct. For multi-document grounded dialog generation, user queries and agent answers are based on top-k retrieved passages. In particular, we generate an initial user query from a single document source and generate the agent answer from top-k passages retrieved on the initial user query. Subsequent user queries and all agent answers are grounded on the retrieved passages and dialog history. We use a series of carefully designed prompts to ensure generated agent answers continue to remain meaningful in the presence of retrieved passages, often noisier than human generated documents. We use MIXTRAL-8X7B-INSTRUCT as our language model for both data generation and LLM-asa-Judge throughout this paper. However, the proposed synthetic data generation framework is not tied to any specific language model. An overview of the proposed technique is shown in Figure 1. To assess the quality of the synthetic data, we carry out extensive human evaluations on 294 diarXiv:2409.11500v1 [cs.CL] 17 Sep 2024

**生成时间**: 2026-04-11 11:20:59  
**源文件**: multiturn_2409.11500.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | Multi-Document Grounded Multi-Turn Synthetic Dialog Generation Young-Suk Lee∗, Chulaka Gunasekara*, Danish Contractor Ramón Fernandez Astudillo, Radu Florian {ysuklee,raduf}@us.ibm.com {Chulaka.Gunasekara,Danish.Contractor,ramon.astudillo}@ibm.com IBM Research AI Abstract We introduce a technique for multi-documentgrounded multi-turn synthetic dialog generation that incorporates three main ideas. First, we control the overall dialog flow using taxonomy-driven user queries that are generated with Chain-of-Thought (CoT) prompting. Second, we support the generation of multidocument-grounded dialogs by mimicking realworld use of retrievers to update the grounding documents after every user-turn in the dialog. Third, we apply LLM-as-a-Judge to filter out queries with incorrect answers. Human evaluation of the synthetic dialog data suggests that the data is diverse, coherent, and includes mostly correct answers. Both human and automatic evaluations of answerable queries indicate that models fine-tuned on synthetic dialogs consistently out-perform those fine-tuned on existing human generated training data across four publicly available multi-turn document grounded benchmark test sets. 1 Introduction As instruction-tuned language models have proven highly effective to generalize to new tasks, (Chung et al., 2022; Wei et al., 2021; Ouyang et al., 2022; Mishra et al., 2022; Wang et al., 2022b), there has been growing interest to acquire synthetic data sets generated from pre-trained language models with a minimal or no human supervision, (Honovich et al., 2022; Wang et al., 2023; Xu et al., 2023; Lee et al., 2023). While there has been an exploration of synthetic data generation for persona-grounded dialog generation (Jang et al., 2022; Bao et al., 2023), topic-grounded dialog generation (Abbasiantaeb et al., 2024), summary-grounded dialog generation (Gunasekara et al., 2021) and conversation generation from dialog scenarios (Mohapatra et al., 2021), the study of methods for generating multidocument grounded multi-turn dialogs by mimicking real-world deployments with retrievers has *Primary authors with equal contributions been limited (Bao et al., 2023; Abbasiantaeb et al., 2024). Generating a realistic multi-turn document grounded dialog can be challenging as one needs to ensure that the (i) the questions are diverse (ii) conversation flows naturally - i.e, it is coherent and the follow-up questions are more than just a collection of question-answer pairs with co-references (iii) the responses are faithful to the documents in the retrieved set and not generated from model parameters. In this paper, we address these challenges and present a technique for generating high quality document grounded multi-turn synthetic dialogs. Our data generation flow incorporates two novel ideas - first, we control the dialog flow according to a question-taxonomy with chain-of-thought (CoT) prompting for query generation. Second, we generate dialogs grounded on both single and multiple retrieved documents. We also apply self-consistency (Wang et al., 2022a) and an LLM-as-judge (Zheng et al., 2023) to ensure the answers are correct. For multi-document grounded dialog generation, user queries and agent answers are based on top-k retrieved passages. In particular, we generate an initial user query from a single document source and generate the agent answer from top-k passages retrieved on the initial user query. Subsequent user queries and all agent answers are grounded on the retrieved passages and dialog history. We use a series of carefully designed prompts to ensure generated agent answers continue to remain meaningful in the presence of retrieved passages, often noisier than human generated documents. We use MIXTRAL-8X7B-INSTRUCT as our language model for both data generation and LLM-asa-Judge throughout this paper. However, the proposed synthetic data generation framework is not tied to any specific language model. An overview of the proposed technique is shown in Figure 1. To assess the quality of the synthetic data, we carry out extensive human evaluations on 294 diarXiv:2409.11500v1 [cs.CL] 17 Sep 2024 |
| **中文标题** | [待翻译] |
| **期刊** | EMNLP |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 17 Sep 2024 |
| **发表日期** | 未知 |


**百分比数据:** 90, 83.6, 73.1, 70, 66, 42, 7.4, 5

**浓度数据:** 2M, 0.807M, 0.453M, 0.613M, 0.013M, 0.478M

**温度条件:** 021, 020°C

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

*报告生成时间：2026-04-11 11:20:59*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
