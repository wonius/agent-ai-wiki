# 深度阅读报告：StruQ: Defending Against Prompt Injection with Structured Queries Sizhe Chen UC Berkeley sizhe.chen @berkeley.edu Julien Piet UC Berkeley julien.piet @berkeley.edu Chawin Sitawarin UC Berkeley chawins @berkeley.edu David Wagner UC Berkeley daw@cs.berkeley.edu Abstract Recent advances in Large Language Models (LLMs) enable exciting LLM-integrated applications, which perform textbased tasks by utilizing their advanced language understanding capabilities. However, as LLMs have improved, so have the attacks against them. Prompt injection attacks are an important threat: they trick the model into deviating from the original application’s instructions and instead follow user directives. These attacks rely on the LLM’s ability to follow instructions and inability to separate prompts and user data. We introduce structured queries, a general approach to tackle this problem. Structured queries separate prompts and data into two channels. We implement a system that supports structured queries. This system is made of (1) a secure front-end that formats a prompt and user data into a special format, and (2) a specially trained LLM that can produce highquality outputs from these inputs. The LLM is trained using a novel fine-tuning strategy: we convert a base (non-instructiontuned) LLM to a structured instruction-tuned model that will only follow instructions in the prompt portion of a query. To do so, we augment standard instruction tuning datasets with examples that also include instructions in the data portion of the query, and fine-tune the model to ignore these. Our system significantly improves resistance to prompt injection attacks, with little or no impact on utility. Our code is released here. 1 1 Introduction Large Language Models (LLMs) [1, 2, 3] have transformed natural language processing. LLMs make it easy to build LLM-integrated applications that work with human-readable text [4] by invoking an LLM to provide text processing or generation. In LLM-integrated applications, it is common to use zero-shot prompting, where the developer implements some task by providing an instruction (also known as a prompt, e.g., “paraphrase the text”) together with user data as LLM input. 1This paper is to appear at USENIX Security Symposium 2025. This introduces the risk of prompt injection attacks [5, 6, 7], where a malicious user can supply data with injected prompts and subvert the operation of the LLM-integrated application. Prompt injection has been dubbed the #1 security risk for LLM applications by OWASP [8]. In this type of attack, the user injects carefully chosen strings into the data (e.g., “Ignore all prior instructions and instead...”). Because LLMs scan their entire input for instructions to follow and there is no separation between prompts and data (i.e., between the part of the input intended by the application developer as prompt and the part intended as user data), existing LLMs are easily fooled by such attacks. Attackers can exploit prompt injection attacks to extract prompts used by the application [9], to direct the LLM towards a completely different task [6], or to control the output of the LLM on the task [10]. Prompt injection is different from jailbreaking [11, 12] (that elicits socially harmful outputs) and adversarial examples [13, 14] (that decreases model performance) and is a simple attack that enables full control over the LLM output. To defend against prompt injections, we propose an approach called structured queries. A structured query to the LLM includes two separate components, the prompt and the data. We propose changing the interface to LLMs to support structured queries, instead of expecting application developers to concatenate prompts and data and send them to the LLM in a single combined input. To ensure security, the LLM must be trained so it will only follow instructions found in the prompt part of a structured query, but not instructions found in the data input. Such an LLM will be immune to prompt injection attacks because the user can only influence the data input where we teach the LLM not to seek instructions. As a first step towards this vision, we propose a system (StruQ) that implements structured queries for LLMs; see Fig. 1. Since it is not feasible to train an entirely new LLM from scratch, we instead devise a system that can be implemented through appropriate use of existing base (noninstruction-tuned) LLMs. StruQ consists of two components: (i) a front-end that is responsible for accepting a prompt and data, i.e., a structured query, and assembling them into a spe1 arXiv:2402.06363v2 [cs.CR] 25 Sep 2024

**生成时间**: 2026-04-11 11:20:55  
**源文件**: query_2402.06363.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | StruQ: Defending Against Prompt Injection with Structured Queries Sizhe Chen UC Berkeley sizhe.chen @berkeley.edu Julien Piet UC Berkeley julien.piet @berkeley.edu Chawin Sitawarin UC Berkeley chawins @berkeley.edu David Wagner UC Berkeley daw@cs.berkeley.edu Abstract Recent advances in Large Language Models (LLMs) enable exciting LLM-integrated applications, which perform textbased tasks by utilizing their advanced language understanding capabilities. However, as LLMs have improved, so have the attacks against them. Prompt injection attacks are an important threat: they trick the model into deviating from the original application’s instructions and instead follow user directives. These attacks rely on the LLM’s ability to follow instructions and inability to separate prompts and user data. We introduce structured queries, a general approach to tackle this problem. Structured queries separate prompts and data into two channels. We implement a system that supports structured queries. This system is made of (1) a secure front-end that formats a prompt and user data into a special format, and (2) a specially trained LLM that can produce highquality outputs from these inputs. The LLM is trained using a novel fine-tuning strategy: we convert a base (non-instructiontuned) LLM to a structured instruction-tuned model that will only follow instructions in the prompt portion of a query. To do so, we augment standard instruction tuning datasets with examples that also include instructions in the data portion of the query, and fine-tune the model to ignore these. Our system significantly improves resistance to prompt injection attacks, with little or no impact on utility. Our code is released here. 1 1 Introduction Large Language Models (LLMs) [1, 2, 3] have transformed natural language processing. LLMs make it easy to build LLM-integrated applications that work with human-readable text [4] by invoking an LLM to provide text processing or generation. In LLM-integrated applications, it is common to use zero-shot prompting, where the developer implements some task by providing an instruction (also known as a prompt, e.g., “paraphrase the text”) together with user data as LLM input. 1This paper is to appear at USENIX Security Symposium 2025. This introduces the risk of prompt injection attacks [5, 6, 7], where a malicious user can supply data with injected prompts and subvert the operation of the LLM-integrated application. Prompt injection has been dubbed the #1 security risk for LLM applications by OWASP [8]. In this type of attack, the user injects carefully chosen strings into the data (e.g., “Ignore all prior instructions and instead...”). Because LLMs scan their entire input for instructions to follow and there is no separation between prompts and data (i.e., between the part of the input intended by the application developer as prompt and the part intended as user data), existing LLMs are easily fooled by such attacks. Attackers can exploit prompt injection attacks to extract prompts used by the application [9], to direct the LLM towards a completely different task [6], or to control the output of the LLM on the task [10]. Prompt injection is different from jailbreaking [11, 12] (that elicits socially harmful outputs) and adversarial examples [13, 14] (that decreases model performance) and is a simple attack that enables full control over the LLM output. To defend against prompt injections, we propose an approach called structured queries. A structured query to the LLM includes two separate components, the prompt and the data. We propose changing the interface to LLMs to support structured queries, instead of expecting application developers to concatenate prompts and data and send them to the LLM in a single combined input. To ensure security, the LLM must be trained so it will only follow instructions found in the prompt part of a structured query, but not instructions found in the data input. Such an LLM will be immune to prompt injection attacks because the user can only influence the data input where we teach the LLM not to seek instructions. As a first step towards this vision, we propose a system (StruQ) that implements structured queries for LLMs; see Fig. 1. Since it is not feasible to train an entirely new LLM from scratch, we instead devise a system that can be implemented through appropriate use of existing base (noninstruction-tuned) LLMs. StruQ consists of two components: (i) a front-end that is responsible for accepting a prompt and data, i.e., a structured query, and assembling them into a spe1 arXiv:2402.06363v2 [cs.CR] 25 Sep 2024 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** | 10.1016/j.infsof.2008.08.002. |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 25 Sep 2024 |
| **发表日期** | 未知 |


**百分比数据:** 100, 99.5, 99, 97, 96, 94, 93, 92

**温度条件:** 32°C

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

*报告生成时间：2026-04-11 11:20:55*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
