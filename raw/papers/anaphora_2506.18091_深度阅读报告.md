# 深度阅读报告：arXiv:2506.18091v1 [cs.CL] 22 Jun 2025 Evaluating Prompt-Based and Fine-Tuned Approaches to Czech Anaphora Resolution Patrik Stano and Aleš Horák[0000−0001−6348−109X] NLP Centre, Faculty of Informatics, Masaryk University Botanická 68a, Brno, Czech Republic nlp.fi.muni.cz/en Abstract. Anaphora resolution plays a critical role in natural language understanding, especially in morphologically rich languages like Czech. This paper presents a comparative evaluation of two modern approaches to anaphora resolution on Czech text: prompt engineering with large language models (LLMs) and fine-tuning compact generative models. Using a dataset derived from the Prague Dependency Treebank, we evaluate several instruction-tuned LLMs, including Mistral Large 2 and Llama 3, using a series of prompt templates. We compare them against finetuned variants of the mT5 and Mistral models that we trained specifically for Czech anaphora resolution. Our experiments demonstrate that while prompting yields promising few-shot results (up to 74.5% accuracy), the fine-tuned models, particularly mT5-large, outperform them significantly, achieving up to 88% accuracy while requiring fewer computational resources. We analyze performance across different anaphora types, antecedent distances, and source corpora, highlighting key strengths and trade-offs of each approach. Keywords: anaphora resolution, sequence-to-sequence models, finetuning, prompt engineering 1 Introduction The use of pronouns to refer to previously mentioned content, a linguistic phenomenon known as anaphora, adds significant complexity to semantic interpretation in natural language processing (NLP). Resolving anaphoric links between pronouns and their antecedents is a crucial step for achieving coherent understanding in tasks such as machine translation, information retrieval, and text generation. Without resolving these references, systems cannot accurately track entities across discourse, which negatively impacts downstream applications. This challenge is even more pronounced in morphologically rich languages like Czech, where grammatical properties such as case, gender, and number are encoded in word forms. Resolving anaphoric references therefore requires not only syntactic and semantic understanding, but also the ability to handle complex inflectional paradigms. For example, correctly translating a pronoun requires knowing the gender of its antecedent, which may not be straightforward from the surrounding context.

**生成时间**: 2026-04-11 11:20:37  
**源文件**: anaphora_2506.18091.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | arXiv:2506.18091v1 [cs.CL] 22 Jun 2025 Evaluating Prompt-Based and Fine-Tuned Approaches to Czech Anaphora Resolution Patrik Stano and Aleš Horák[0000−0001−6348−109X] NLP Centre, Faculty of Informatics, Masaryk University Botanická 68a, Brno, Czech Republic nlp.fi.muni.cz/en Abstract. Anaphora resolution plays a critical role in natural language understanding, especially in morphologically rich languages like Czech. This paper presents a comparative evaluation of two modern approaches to anaphora resolution on Czech text: prompt engineering with large language models (LLMs) and fine-tuning compact generative models. Using a dataset derived from the Prague Dependency Treebank, we evaluate several instruction-tuned LLMs, including Mistral Large 2 and Llama 3, using a series of prompt templates. We compare them against finetuned variants of the mT5 and Mistral models that we trained specifically for Czech anaphora resolution. Our experiments demonstrate that while prompting yields promising few-shot results (up to 74.5% accuracy), the fine-tuned models, particularly mT5-large, outperform them significantly, achieving up to 88% accuracy while requiring fewer computational resources. We analyze performance across different anaphora types, antecedent distances, and source corpora, highlighting key strengths and trade-offs of each approach. Keywords: anaphora resolution, sequence-to-sequence models, finetuning, prompt engineering 1 Introduction The use of pronouns to refer to previously mentioned content, a linguistic phenomenon known as anaphora, adds significant complexity to semantic interpretation in natural language processing (NLP). Resolving anaphoric links between pronouns and their antecedents is a crucial step for achieving coherent understanding in tasks such as machine translation, information retrieval, and text generation. Without resolving these references, systems cannot accurately track entities across discourse, which negatively impacts downstream applications. This challenge is even more pronounced in morphologically rich languages like Czech, where grammatical properties such as case, gender, and number are encoded in word forms. Resolving anaphoric references therefore requires not only syntactic and semantic understanding, but also the ability to handle complex inflectional paradigms. For example, correctly translating a pronoun requires knowing the gender of its antecedent, which may not be straightforward from the surrounding context. |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** | 10.18653/v1 |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 22 Jun 2025 |
| **发表日期** | 未知 |


**百分比数据:** 88, 74.5, 13.9

**浓度数据:** 0.880M, 5m, 0.2m, 0.965M, 3M, 0.310M

**温度条件:** 022, 021°C

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

*报告生成时间：2026-04-11 11:20:37*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
