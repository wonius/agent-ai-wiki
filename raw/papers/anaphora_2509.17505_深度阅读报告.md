# 深度阅读报告：CorefInst: Leveraging LLMs for Multilingual Coreference Resolution Tu˘gba Pamay Arslan⋄and Emircan Erol⋄and Gül¸sen Eryi˘git⋄ ⋄˙ITÜ NLP Research Group Department of Artificial Intelligence and Data Engineering ˙Istanbul Technical University [pamay, erole20, gulsen.cebiroglu]@itu.edu.tr Abstract Coreference Resolution (CR) is a crucial yet challenging task in natural language understanding, often constrained by task-specific architectures and encoder-based language models that demand extensive training and lack adaptability. This study introduces the first multilingual CR methodology which leverages decoder-only LLMs to handle both overt and zero mentions. The article explores how to model the CR task for LLMs via five different instruction sets using a controlled inference method. The approach is evaluated across three LLMs; Llama 3.1, Gemma 2, and Mistral 0.3. The results indicate that LLMs, when instruction-tuned with a suitable instruction set, can surpass state-of-theart task-specific architectures. Specifically, our best model, a fully fine-tuned Llama 3.1 for multilingual CR, outperforms the leading multilingual CR model (i.e., Corpipe 24 single stage variant) by 2 pp on average across all languages in the CorefUD v1.2 dataset collection. 1 Introduction Coreference Resolution (CR) is a semantic-level task in natural language processing (NLP) that involves detecting and clustering mentions referring to the same entity within a text. An end-to-end CR system consists of two phases: mention detection and coreference linking. In mention detection, all possible referential mentions are extracted from the text. In the next step, mentions referring to the same entity are collected under the same cluster, by resolving which mentions are coreferential. This task is crucial for various higherlevel NLP applications, including machine translation (Stojanovski and Fraser, 2018), text summarization (Liu et al., 2021), and question answering systems (Bhattacharjee et al., 2020), as it helps in understanding the coherence and context of the text. Large Language Models (LLMs) are traditionally classified into three categories: encoder-only models (e.g., BERT), encoder-decoder models (e.g., T5), and decoder-only models (e.g. GPT, Llama). Initial task-specific neural CR models (Lee et al., 2017; Straka, 2023) incorporate an encoder-only LLM at the bottom layer to generate word embeddings, followed by a specialized neural architecture where the LLM layer is also fine-tuned during the training process, illustrated in Figure 1. However, task-specific models require specialized architectures, vast amount of training data, and an extensive fine-tuning/training process from scratch, leading to limited generalizability and flexibility across new datasets, languages, tasks, and domains. Furthermore, due to their niche nature, these models cannot exploit decoder-based LLMs directly, which have been rapidly developing. On the other hand, instruction tuning has emerged as a powerful paradigm, enabling decoder-only LLMs to adapt to diverse tasks through carefully crafted prompts or instructions, thereby addressing these limitations. It is important to note that recently, LLMs are often used interchangeably with decoder-only models in the literature, particularly due to the popularity of autoregressive models like GPT. From this point onward in the article, we will refer to decoder-only LLMs simply as LLMs. Despite notable progress in CR, the technologies developed to tackle challenges such as handling zero mentions and generalizing across various languages are still evolving (Novák et al., 2024; Pamay Arslan and Eryi˘git, 2025). Recent CR studies (Chen et al., 2021; Straka, 2023; Pamay Arslan and Eryi˘git, 2025) address zero mentions and their coreferential relations alongside overt mentions. Zero mentions are not explicitly defined in an original sentence, and result from dropped-pronouns in pro-dropped languages (e.g., Chinese, Hungarians, Turkish). Figure 2 presents a sample Czech sentence containing zero mentions along with its arXiv:2509.17505v1 [cs.CL] 22 Sep 2025

**生成时间**: 2026-04-11 11:20:13  
**源文件**: anaphora_2509.17505.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | CorefInst: Leveraging LLMs for Multilingual Coreference Resolution Tu˘gba Pamay Arslan⋄and Emircan Erol⋄and Gül¸sen Eryi˘git⋄ ⋄˙ITÜ NLP Research Group Department of Artificial Intelligence and Data Engineering ˙Istanbul Technical University [pamay, erole20, gulsen.cebiroglu]@itu.edu.tr Abstract Coreference Resolution (CR) is a crucial yet challenging task in natural language understanding, often constrained by task-specific architectures and encoder-based language models that demand extensive training and lack adaptability. This study introduces the first multilingual CR methodology which leverages decoder-only LLMs to handle both overt and zero mentions. The article explores how to model the CR task for LLMs via five different instruction sets using a controlled inference method. The approach is evaluated across three LLMs; Llama 3.1, Gemma 2, and Mistral 0.3. The results indicate that LLMs, when instruction-tuned with a suitable instruction set, can surpass state-of-theart task-specific architectures. Specifically, our best model, a fully fine-tuned Llama 3.1 for multilingual CR, outperforms the leading multilingual CR model (i.e., Corpipe 24 single stage variant) by 2 pp on average across all languages in the CorefUD v1.2 dataset collection. 1 Introduction Coreference Resolution (CR) is a semantic-level task in natural language processing (NLP) that involves detecting and clustering mentions referring to the same entity within a text. An end-to-end CR system consists of two phases: mention detection and coreference linking. In mention detection, all possible referential mentions are extracted from the text. In the next step, mentions referring to the same entity are collected under the same cluster, by resolving which mentions are coreferential. This task is crucial for various higherlevel NLP applications, including machine translation (Stojanovski and Fraser, 2018), text summarization (Liu et al., 2021), and question answering systems (Bhattacharjee et al., 2020), as it helps in understanding the coherence and context of the text. Large Language Models (LLMs) are traditionally classified into three categories: encoder-only models (e.g., BERT), encoder-decoder models (e.g., T5), and decoder-only models (e.g. GPT, Llama). Initial task-specific neural CR models (Lee et al., 2017; Straka, 2023) incorporate an encoder-only LLM at the bottom layer to generate word embeddings, followed by a specialized neural architecture where the LLM layer is also fine-tuned during the training process, illustrated in Figure 1. However, task-specific models require specialized architectures, vast amount of training data, and an extensive fine-tuning/training process from scratch, leading to limited generalizability and flexibility across new datasets, languages, tasks, and domains. Furthermore, due to their niche nature, these models cannot exploit decoder-based LLMs directly, which have been rapidly developing. On the other hand, instruction tuning has emerged as a powerful paradigm, enabling decoder-only LLMs to adapt to diverse tasks through carefully crafted prompts or instructions, thereby addressing these limitations. It is important to note that recently, LLMs are often used interchangeably with decoder-only models in the literature, particularly due to the popularity of autoregressive models like GPT. From this point onward in the article, we will refer to decoder-only LLMs simply as LLMs. Despite notable progress in CR, the technologies developed to tackle challenges such as handling zero mentions and generalizing across various languages are still evolving (Novák et al., 2024; Pamay Arslan and Eryi˘git, 2025). Recent CR studies (Chen et al., 2021; Straka, 2023; Pamay Arslan and Eryi˘git, 2025) address zero mentions and their coreferential relations alongside overt mentions. Zero mentions are not explicitly defined in an original sentence, and result from dropped-pronouns in pro-dropped languages (e.g., Chinese, Hungarians, Turkish). Figure 2 presents a sample Czech sentence containing zero mentions along with its arXiv:2509.17505v1 [cs.CL] 22 Sep 2025 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 22 Sep 2025 |
| **发表日期** | 未知 |


**关键指标:**
- R²: 16

**百分比数据:** 76.15, 73.82, 72.46, 1.4, 1.3, 1.1, 0.9

**浓度数据:** 20m, 3m, 2m, 10m, 13M, 1M

**温度条件:** 023°C

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

*报告生成时间：2026-04-11 11:20:13*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
