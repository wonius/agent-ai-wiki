# 深度阅读报告：Preprint. Under review. SkillRouter: Skill Routing for LLM Agents at Scale YanZhao Zheng ZhenTao Zhang Chao Ma YuanQiang Yu JiHuai Zhu Wu Yong Tianze Xu Baohua Dong∗Hangcheng Zhu Ruohui Huang Gang Yu Alibaba Group, Hangzhou, China zhengyanzhao.zyz, zhangzhentao.zzt, mc524716, yuyuanqiang.yyq, zhujihuai.zjh, wy517954, xutianze.xtz, baohua.dbh, linran.lr09, wentong, ruohai@alibaba-inc.com Abstract Reusable skills let LLM agents package task-specific procedures, tool affordances, and execution guidance into modular building blocks. As skill ecosystems grow to tens of thousands of entries, exposing every skill at inference time becomes infeasible. This creates a skill-routing problem: given a user task, the system must identify relevant skills before downstream planning or execution. Existing agent stacks often rely on progressive disclosure, exposing only skill names and descriptions while hiding the full implementation body. We examine this design choice on a SkillsBenchderived benchmark with approximately 80K candidate skills, targeting the practically important setting of large skill registries with heavy overlap. Across representative sparse, dense, and reranking baselines on this setting, hiding the skill body causes a 31–44 percentage point drop in routing accuracy, showing that full skill text is a critical routing signal in this setting rather than a minor metadata refinement. Motivated by this finding, we present SKILLROUTER1, a compact 1.2B full-text retrieve-andrerank pipeline. SKILLROUTER achieves 74.0% Hit@1 on our benchmark— the strongest average top-1 routing performance among the baselines we evaluate—while using 13× fewer parameters and running 5.8× faster than the strongest base pipeline. The ranking gains further generalize to a supplementary benchmark independently constructed from three skill sources. In a complementary end-to-end study across four coding agents, routing gains transfer to improved task success, with larger gains for more capable agents. 1 Introduction Skills have emerged as a practical abstraction for extending LLM agents with reusable procedures, tool knowledge, and execution guidance. Recent coding-agent products such as Claude Code, Codex, and OpenClaw expose reusable skills as a first-class capability (Anthropic, 2025; OpenAI, 2025; OpenClaw, 2026b). These systems reflect the growing use of skill registries in real deployments. Presenting every skill to the agent is infeasible, so real systems need skill routing: retrieving the right skill from a large pool given a user task. This setting has an important asymmetry: the routing component can inspect the full skill text, while the agent that eventually consumes the skill usually sees only its name and description. In deployed agent stacks, this upstream routing decision is a high-leverage bottleneck: once the wrong skill shortlist is surfaced, downstream planning and execution ∗Corresponding author. 1https://github.com/zhengyanzhao1997/SkillRouter 1 arXiv:2603.22455v4 [cs.LG] 1 Apr 2026

**生成时间**: 2026-04-11 11:21:12  
**源文件**: SkillRouter_2603.22455.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | Preprint. Under review. SkillRouter: Skill Routing for LLM Agents at Scale YanZhao Zheng ZhenTao Zhang Chao Ma YuanQiang Yu JiHuai Zhu Wu Yong Tianze Xu Baohua Dong∗Hangcheng Zhu Ruohui Huang Gang Yu Alibaba Group, Hangzhou, China zhengyanzhao.zyz, zhangzhentao.zzt, mc524716, yuyuanqiang.yyq, zhujihuai.zjh, wy517954, xutianze.xtz, baohua.dbh, linran.lr09, wentong, ruohai@alibaba-inc.com Abstract Reusable skills let LLM agents package task-specific procedures, tool affordances, and execution guidance into modular building blocks. As skill ecosystems grow to tens of thousands of entries, exposing every skill at inference time becomes infeasible. This creates a skill-routing problem: given a user task, the system must identify relevant skills before downstream planning or execution. Existing agent stacks often rely on progressive disclosure, exposing only skill names and descriptions while hiding the full implementation body. We examine this design choice on a SkillsBenchderived benchmark with approximately 80K candidate skills, targeting the practically important setting of large skill registries with heavy overlap. Across representative sparse, dense, and reranking baselines on this setting, hiding the skill body causes a 31–44 percentage point drop in routing accuracy, showing that full skill text is a critical routing signal in this setting rather than a minor metadata refinement. Motivated by this finding, we present SKILLROUTER1, a compact 1.2B full-text retrieve-andrerank pipeline. SKILLROUTER achieves 74.0% Hit@1 on our benchmark— the strongest average top-1 routing performance among the baselines we evaluate—while using 13× fewer parameters and running 5.8× faster than the strongest base pipeline. The ranking gains further generalize to a supplementary benchmark independently constructed from three skill sources. In a complementary end-to-end study across four coding agents, routing gains transfer to improved task success, with larger gains for more capable agents. 1 Introduction Skills have emerged as a practical abstraction for extending LLM agents with reusable procedures, tool knowledge, and execution guidance. Recent coding-agent products such as Claude Code, Codex, and OpenClaw expose reusable skills as a first-class capability (Anthropic, 2025; OpenAI, 2025; OpenClaw, 2026b). These systems reflect the growing use of skill registries in real deployments. Presenting every skill to the agent is infeasible, so real systems need skill routing: retrieving the right skill from a large pool given a user task. This setting has an important asymmetry: the routing component can inspect the full skill text, while the agent that eventually consumes the skill usually sees only its name and description. In deployed agent stacks, this upstream routing decision is a high-leverage bottleneck: once the wrong skill shortlist is surfaced, downstream planning and execution ∗Corresponding author. 1https://github.com/zhengyanzhao1997/SkillRouter 1 arXiv:2603.22455v4 [cs.LG] 1 Apr 2026 |
| **中文标题** | [待翻译] |
| **期刊** | Science |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 1 Apr 2026 |
| **发表日期** | 未知 |


**关键指标:**
- R²: 0.04

**百分比数据:** 100, 99.6, 98.1, 98, 97.3, 96.5, 96, 94

**浓度数据:** 5m, 5.2M, 4m, 5.4m, 495.8m, 335M

**温度条件:** 50, 44, 20°C

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

*报告生成时间：2026-04-11 11:21:12*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
