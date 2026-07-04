# 上下文学习 / Skill 训练领域相关素材

## 说明

本文件收录与 "静态上下文训练"（ContextLearning, CL）相关的现有产品、论文、工具和思想，作为 CL 项目的参照系和素材来源。按与 CL 的核心命题——"将上下文本体视为可训练参数，通过非参数方式逼近目标行为"——的相关性组织。

最后更新: 2026-06-30

---

## 1. SkillOpt 家族：最直接的相关工作

这批工作（全部来自 2026年5-6月）明确将 skill 文档视为可训练的外部参数，用 ML 训练概念来优化技能文本。

### 1.1 SkillOpt

- **来源**: Microsoft Research + 上海交大 + 复旦
- **类型**: 论文 + 开源
- **链接**: https://arxiv.org/abs/2605.23904 | https://github.com/microsoft/skillopt
- **核心思想**: 将 Skill 文档视为冻结模型的 **可训练外部状态**。用独立优化器对 skill 文档做增删改编辑，仅在验证集提升时接受。引入 textual learning-rate budget、rejected-edit buffer、epoch-wise slow/meta update。
- **关键引文**: "the skill should instead be trained as the external state of a frozen agent, with the same discipline that makes weight-space optimization reproducible"
- **中文传播**: "Skill 即参数，执行即前向传播"（微信 · Walter Sun Tech）；"文档即参数"（什么值得买）
- **对 CL 的意义**: 直接验证了 CL 的核心假设。SkillOpt 是"训练步骤"的一种自动化实现。

### 1.2 SkillGrad

- **来源**: Wang 等
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2605.27760
- **核心思想**: 将技能包视为 **结构化参数**，以 **梯度下降方式** 优化。任务执行提供轨迹级别的 loss 证据，自动诊断提供文本形式的梯度，动量智能体累积重复诊断模式。
- **关键引文**: "treats the skill package as a structured parameter to optimize in a gradient descent fashion"
- **对 CL 的意义**: 最明确的梯度下降类比。动量智能体 = CL 训练信号累积的参考实现。

### 1.3 ParametricSkills

- **来源**: Zhao 等
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2606.30015
- **核心思想**: 用超网络将 **文本技能转化为 LoRA 适配器**（测试时参数化），实现"无上下文的技能利用"。技能可累积，支持测试时持续学习。
- **对 CL 的意义**: 桥接了"外部文本"与"内部参数"两条路线。技能可以参数化累积。

### 1.4 LatentSkill

- **来源**: Yu 等
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2606.06087
- **核心思想**: 将技能知识从上下文空间转移到 **权重空间**。预训练超网络将文本技能转换为 LoRA，生成的技能 LoRA 形成结构化语义几何，可通过 **参数空间算术** 组合。
- **对 CL 的意义**: 技能组合 ≈ 模型合并/集成的直接实现。证明了权重空间技能比上下文空间技能更高效（-64.1% prefill tokens）。

### 1.5 PEAM (Parametric Embodied Agent Memory)

- **来源**: Guo 等
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2605.27762
- **核心思想**: 将 memory 从推理时检索转变为 **参数驻留技能**。MoE-LoRA 架构，按类别物理隔离适配器。**将失败视为一等训练信号**。引入参数化价值度评分和自触发整合。
- **对 CL 的意义**: memory 的"参数化价值"判断——什么经验值得写成技能。失败 = 训练的 first-class signal。

### 1.6 HiSME (Hierarchical Skill Meta-Evolving)

- **来源**: Li 等
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2605.28390
- **核心思想**: 联合优化 **技能** 和 **技能进化策略**，从执行轨迹中学习 **元技能**。
- **对 CL 的意义**: CL 的"训练流程"本身也可以被训练。元技能 = 如何训练技能的技能。

### 1.7 总结：SkillOpt 家族的演进

```text
SkillOpt (外部参数优化)
  ├── SkillGrad (梯度类比 + 动量)
  ├── ParametricSkills (文本 → LoRA 参数)
  ├── LatentSkill (权重空间技能 + 参数算术组合)
  ├── PEAM (memory → 参数驻留技能)
  └── HiSME (元技能：优化进化策略)
```

---

## 2. 提示词优化框架（Prompt as Parameter）

这批工作早于 SkillOpt，同样是"将提示词视为可优化参数"，但通常不限于 skill 格式。它们是 CL 的重要技术前身。

### 2.1 DSPy

- **来源**: Stanford NLP (Khattab 等)
- **类型**: 论文 + 开源
- **链接**: https://github.com/stanfordnlp/dspy | arXiv:2310.03714
- **核心思想**: 将 LM 程序（chain、RAG、agent）声明式定义，通过 compiler 自动优化 prompt 和 few-shot。优化器 (MIPROv2, COPRO, GEPA) 从标注数据中 tune prompt 结构。
- **对 CL 的意义**: 最成熟的"prompt 可训练"框架。CL 的流程设计可参考 DSPy 的优化器架构。GEPA (Genetic Evolution of Prompts) 尤其接近 skill 进化场景。

### 2.2 TextGrad

- **来源**: Zou 等 (ICLR 2025)
- **类型**: 论文 + 开源
- **链接**: https://github.com/zou-group/textgrad
- **核心思想**: **自然语言自动微分**。将文本视为可微变量，LLM 生成的文本批评作为"梯度"来更新 prompt。
- **对 CL 的意义**: "文本梯度"的概念——直接类比 backprop。可用于 CL 训练信号的生成。

### 2.3 OPRO (Optimization by PROmpting)

- **来源**: Google DeepMind (Yang 等, ICLR 2024)
- **类型**: 论文 + 开源
- **链接**: https://arxiv.org/abs/2309.03409 | https://github.com/google-deepmind/opro
- **核心思想**: **LLM 作为优化器**。LLM 接收当前 prompt + 分数，生成更好的候选。
- **对 CL 的意义**: 最简洁的 prompt 优化范式——一个 LLM 调用 = 一步训练。

### 2.4 SePO (Self-Evolving Prompt Agent)

- **来源**: Tao 等 (2026)
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2606.04465
- **核心思想**: 将 **优化器自身的 system prompt** 也作为优化目标。自指设计：prompt agent 进化自己的进化策略。
- **对 CL 的意义**: CL 的"训练流程"可能也需要自我进化。

### 2.5 TextReg

- **来源**: Fu 等 (2026)
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2605.21318
- **核心思想**: 研究 **prompt 分布过拟合**——prompt 积累窄样本特定规则。提出 Dual-Evidence Gradient Purification 和 Semantic Edit Regularization。
- **对 CL 的意义**: CL 的"过拟合"概念在学术文献中的直接对应。提供了正则化技术参考。

### 2.6 MPO (Modular Prompt Optimization)

- **来源**: Sharma, Henley (2026)
- **类型**: 论文
- **链接**: https://arxiv.org/abs/2601.04055
- **核心思想**: 将 prompt 视为结构化对象（固定语义区段），对每个区段独立应用 **区间局部文本梯度**。
- **对 CL 的意义**: 类比于神经网络的 layer-wise 优化。CL 的上下文也可以分区训练。

### 2.7 总结：Prompt 优化谱系

```text
OPRO (LLM as optimizer, 2023)
  └── DSPy (programmatic compilation, 2023)
        ├── hermes-agent-self-evolution (DSPy + GEPA on skills, 2025)
        └── MIPROv2 (jointly optimize instruction + few-shot)
TextGrad (textual gradients, 2024)
  ├── TriMem (TextGrad for memory prompts, 2026)
  ├── TextReg (overfitting + regularization, 2026)
  ├── MPO (modular section-local gradients, 2026)
  └── REMO (TextGrad + memory meta-optimization, 2025)
AutoPrompt (gradient-guided discrete search, 2020) [precursor]
```

---

## 3. Agent 技能/记忆自进化系统

这批工作关注 agent 如何从经验中自主获取、提炼和进化技能/记忆。

### 3.1 Voyager

- **来源**: Wang 等 (NeurIPS 2023)
- **链接**: https://arxiv.org/abs/2305.16291
- **核心机制**: 自动课程学习 + 代码技能库 + 迭代式提示机制。Minecraft 场景。技能库持续增长，跨任务可组合。
- **对 CL 的意义**: 开创性工作——技能库作为外部化、可组合的程序性记忆。代码形式比自然语言更可靠。

### 3.2 Reflexion

- **来源**: Shinn 等 (NeurIPS 2023)
- **链接**: https://arxiv.org/abs/2303.11366
- **核心机制**: Actor-Critic 式架构：agent 执行 → 评估 → 失败轨迹压缩为文本记忆 → 注入后续情节。
- **对 CL 的意义**: 最简形式的技能进化：将失败转化为文本记忆。无结构化存储，但验证了反馈闭包的有效性。

### 3.3 Generative Agents

- **来源**: Park 等 (2023)
- **链接**: https://arxiv.org/abs/2304.03442
- **核心机制**: 记忆流（带时间戳观察）→ 检索（相关 + 时效 + 重要）→ 反思（高层次见解）→ 规划。25 agent 模拟。
- **对 CL 的意义**: 最具影响力的记忆架构。**反思** 是从原始经验到可重用知识的压缩步骤。重要性评分的概念。

### 3.4 ExpeL

- **来源**: Zhao 等 (2024)
- **链接**: https://arxiv.org/abs/2408.10635
- **核心机制**: 轨迹 → 经验总结 → 外部经验库。未来任务检索相关经验。
- **对 CL 的意义**: 经验 → 技能的明确压缩步骤。需要过滤和优先级排序。

### 3.5 FORGE

- **来源**: Bogdanov 等 (2026)
- **链接**: https://arxiv.org/abs/2605.16233
- **核心机制**: Reflexion 式内循环 + **群体广播** 外循环。专用反思 agent 将失败轨迹转化为可重用知识工件（规则/示例/混合）。收敛实例通过毕业标准冻结。
- **对 CL 的意义**: 群体广播机制——技能进化可以跨 agent 传递。毕业标准 = CL 的 quality gate。

### 3.6 RSEA (Recursive Self-Evolving Agent)

- **来源**: Nguyen 等 (2026)
- **链接**: https://arxiv.org/abs/2606.28374
- **核心机制**: 三层自然语言状态（Policy / Skill / 规程书）→ 跨代重写 → **仅在留出分割无回归时提交**。
- **对 CL 的意义**: **严格的安全门控**——证明不受约束的上下文进化是高方差且不安全的。留出验证保证单调安全。

### 3.7 UCE (Unified Context Evolution)

- **来源**: Zhu 等 (2026)
- **链接**: https://arxiv.org/abs/2606.02304
- **核心机制**: 四种可进化上下文单元：**记忆 / 策略 / 工作流 / 技能**。每种有类型特定的提取条件、检索、评分和修剪。调度器将生成预算分配给最弱的类型。
- **对 CL 的意义**: 最全面的上下文类型学分类。CL 的"上下文"概念可能也需要内部类型分层。

### 3.8 EDV (Execute-Distill-Verify)

- **来源**: Zhu 等 (2026)
- **链接**: https://arxiv.org/abs/2606.24428
- **核心机制**: 解耦三阶段——多异构 agent 并行探索 → 第三方 agent 比较分析轨迹生成候选 → 执行组通过共识机制验证。
- **对 CL 的意义**: 解决 **自我确认陷阱**（self-confirmation trap）——单一 agent 的错误经验会污染库。多 agent 共识验证。

### 3.9 Memento-Skills

- **来源**: Zhou 等 (2026)
- **链接**: https://arxiv.org/abs/2603.18743
- **核心机制**: 技能路由器（可训练的行为选择）+ 技能库（结构化 markdown）。读阶段：路由器选择最相关技能。写阶段：agent 更新/扩展技能库。
- **对 CL 的意义**: 明确区分 **技能选择** 和 **技能拥有**。路由器需要行为训练。

### 3.10 Metis

- **来源**: Dai 等 (2026)
- **链接**: https://arxiv.org/abs/2606.24151
- **核心机制**: 分层双表示记忆——**文本经验** + 选择性结晶为 **可调用代码工具**。两者互补权衡。
- **对 CL 的意义**: 文本 vs 代码的选择应由经验特征驱动。仅在反复重用时值得结晶为代码。

### 3.11 EvoDS

- **来源**: Yang 等 (KDD 2026)
- **链接**: https://arxiv.org/abs/2606.03841
- **核心机制**: 自主技能获取 (ASA) + 自适应上下文压缩 (ACC)。两阶段训练。理论上证明与信息瓶颈目标对齐。
- **对 CL 的意义**: 技能学习和上下文管理作为联合优化问题。信息瓶颈提供理论依据。

### 3.12 Experience Compression Spectrum

- **来源**: Zhang 等 (2026)
- **链接**: https://arxiv.org/abs/2604.15877
- **核心机制**: 记忆 / 技能 / 规则 在单一压缩轴上定位——记忆压缩 5-20x，程序技能 50-500x，声明规则 1000x+。**20+ 系统的映射显示没有系统支持自适应跨层压缩**（"缺失的对角线"）。
- **对 CL 的意义**: CL 的核心理论框架之一。压缩率 × 可迁移性的权衡——同一经验在不同条件下适合作为记忆、技能或规则。

---

## 4. 理论根基

### 4.1 Scaffolded LMs with Language Supervision

- **来源**: Lin 等 (2024)
- **链接**: https://arxiv.org/abs/2410.16392
- **核心思想**: 将 scaffolded LM（LM + tool + prompt + code）视为 **半参数模型**。prompt/tool/code 是 **非参数变量**。通过语言反馈训练这些非参数变量称为 **"用语言监督训练 scaffolding LM"**。
- **对 CL 的意义**: 最接近 CL 核心论题的已有工作。直接使用了"非参数训练"的框架。
- **关键引文**: "We view scaffolded LMs as semi-parametric models wherein we train non-parametric variables, including the prompt, tools, and scaffold's code... A key feature of non-parametric training is the ability to learn from language."

### 4.2 ICL as Implicit Gradient Descent

- **来源**: Dai 等 (2022); von Oswald 等 (2023); Akyürek 等 (2022)
- **链接**: arXiv:2212.10897 / 2212.10348 / 2211.15661
- **核心思想**: Transformer 的 in-context learning 在数学上等价于对线性化模型的梯度下降。**上下文示例 = 训练数据，注意力更新 = 参数更新**。
- **对 CL 的意义**: CL 的理论基础——上下文修改作为训练操作有理论支撑。

### 4.3 Prompt Tuning / Prefix Tuning

- **来源**: Lester 等 (2021); Li & Liang (2021)
- **链接**: arXiv:2104.08691 / 2101.00190
- **核心思想**: 在输入层 / 每层前拼接 **可学习连续向量**，通过 backprop 优化，冻结 LM。
- **对 CL 的意义**: "外界输入可学习"的 parametric 先例。CL 的区别在于操作对象是离散文本而非连续向量。

### 4.4 安全：Self-Evolving LLM Agent Systems 的攻击面

- **来源**: Lin 等 (2026)
- **链接**: https://arxiv.org/abs/2606.23075
- **核心思想**: Module-Lifecycle Attack Surface (MLAS) 矩阵：5 模块 × 5 生命周期阶段 = 25 单元格。17 个面临关键威胁。自进化将攻击从会话级升级为谱系级。
- **对 CL 的意义**: 技能进化系统的安全设计不可忽视。技能谱系追踪是必要条件。

---

## 5. 开源工具与产品

### 5.1 hermes-agent-self-evolution

- **类型**: 开源
- **链接**: https://github.com/NousResearch/hermes-agent-self-evolution
- **Stars**: 4,419
- **核心**: "Evolutionary self-improvement for Hermes Agent — optimize skills, prompts, and code using DSPy + GEPA"。使用 DSPy optimizer + 遗传进化来进化 agent skill。
- **对 CL 的意义**: 最接近 CL 的已有开源产品。DSPy + GEPA 在 skill 上的直接应用。

### 5.2 TreeSkill / EvoSkill

- **类型**: 开源
- **链接**: https://github.com/JimmyMa99/TreeSkill | JimmyMa99/EvoSkill
- **核心**: "Skill optimization framework for LLMs — evolve system prompts via textual gradient descent with beam search, human-in-the-loop annotation, and DPO preference data export."
- **对 CL 的意义**: 非常直接对应——skill 文档的文本梯度下降。支持 human-in-the-loop 标注和 DPO 导出。

### 5.3 memov

- **类型**: 开源
- **链接**: https://github.com/memovai/memov
- **Stars**: 192
- **核心**: "Give git-like & traceable memory to agents. Self-evolution for skills." Git 式版本化 agent 记忆和技能。
- **对 CL 的意义**: 版本化 + 自进化。

### 5.4 Agent-Loop-Skills

- **类型**: 开源
- **链接**: https://github.com/gaasher/Agent-Loop-Skills
- **Stars**: 114
- **核心**: "Loop until it's better — drop-in agentic loops (autoresearch, code/SQL/prompt optimization, red-teaming) as open-standard Agent Skills. Verification-gated."
- **对 CL 的意义**: "loop until better" + 验证门控。CL 训练循环的参考实现。

### 5.5 awesome-agent-evolution

- **类型**: 汇总列表
- **链接**: https://github.com/Shiyao-Huang/awesome-agent-evolution (⭐169) | https://github.com/EvoMap/awesome-agent-evolution (⭐147)
- **核心**: AI agent evolution / memory / self-improvement 的精选列表。
- **对 CL 的意义**: 重要参考——该领域最全面的公开索引。

### 5.6 SkillEvolBench

- **类型**: 基准测试
- **链接**: https://arxiv.org/abs/2605.24117
- **核心**: 首个专门衡量"经验何时成为技能"的基准。180 个任务，6 个环境。**关键发现**：原始轨迹重复通常优于提炼技能——当前的提炼方法丢弃了有用的上下文。
- **对 CL 的意义**: **直接相关**——CL 需要回答"什么构成了有效技能提炼"。"缺失的上下文"问题可能正是 CL 要解决的核心。

---

## 6. 行业/商业产品

### 6.1 OpenCode Skills

- **链接**: https://opencode.ai/docs/skills/
- **类型**: 商业产品（开源核心）
- **核心**: SKILL.md 文件通过 `skill` 工具按需加载。技能是静态文件，由路径发现。
- **与 CL 的关系**: 定义了"skill 文档"这一 CL 的训练对象。但 opencode 不包含训练/优化循环——CL 是其"训练"侧。

### 6.2 Anthropic (Claude Code Skills)

- **链接**: docs.anthropic.com 相关内容（部分区域受限）
- **核心**: Claude Code 的 skill 系统，强调渐进式披露（3 级详情）、可组合、可移植。
- **与 CL 的关系**: 商业化的 skill 生态。锚定 "skill 作为 onbarding guide" 但未使用 ML 训练类比。

### 6.3 DSPy 生态 (商业使用)

- 许多团队将 DSPy 用于生产环境的 prompt 优化。DSPy 的 compiler 模式是 CL 流程的最接近行业对标。

### 6.4 LangChain Hub / Prompt Management

- LangChain 的 prompt hub 提供版本管理和共享。但重点是托管而非训练。

---

## 7. 关键 Gaps 与启示

### 7.1 CL 的独特位置

| 维度 | 现有工作 | CL 的定位 |
|---|---|---|
| 优化对象 | prompt（DSPy）/ code（Voyager）/ 记忆（Generative Agents） | **任意上下文本体**（skill/prompt/memory/rule） |
| 自动化程度 | 全自动（SkillOpt）/ 半自动（DSPy）/ 手工（OpenCode） | **人的角色和自动化之间可配置** |
| ML 映射 | 字面实现（SkillOpt epoch）/ 类比（CL analogy） | **概念层框架 + 可选机制** |
| 目标发现 | 假设目标已定（SkillOpt）/ 无概念（DSPy） | **从隐式目标到显式目标的提炼**（核心差异） |
| 信号类型 | 单一（assertion loss / metric） | **丰富信号分类学**（偏离/正向/负向/反馈信息） |
| 过拟合处理 | TextReg 等 | **浅学习 / 上下文污染 / 案例风险概念** |

### 7.2 重要启示

1. **验证门控是必须的**（RSEA, EDV）：不受约束的上下文进化不安全。需要留出验证或共识机制。

2. **技能 ≠ 记忆**（UCE, Experience Compression Spectrum）：不同的上下文类型有不同的生命周期和压缩率。CL 需要内部类型学。

3. **文本 vs 代码**（Metis）：两者互补。文本灵活但噪声大，代码可靠但成本高。仅在反复重用时结晶为代码。

4. **"丢失的对角线"**（Experience Compression Spectrum）：没有系统支持自适应跨压缩水平的上下文管理——CL 有机会占据这个位置。

5. **自我确认陷阱**（EDV）：单一 agent 的自我反思可能固化错误。多 agent 验证或外部裁判是方案。

6. **元进化**（HiSME, SePO）：进化策略本身也可以被优化。CL 的流程需要支持流程本身的迭代。

7. **SkillEvolBench 的核心问题**"当前提炼方法丢弃了有用上下文"正是 CL 要解决的问题——CL 的训练信号分类学提供了解决方案的词汇。

8. **进化安全**（Lin 2026）：技能谱系追踪和安全审计是落地前必须考虑的。

### 7.3 建议优先阅读

```text
必读:
  1. Scaffolded LMs survey (Lin 2024, arXiv:2410.16392) — 最接近的理论框架
  2. SkillOpt (arXiv:2605.23904) — 最直接的自动化实现
  3. Experience Compression Spectrum (arXiv:2604.15877) — 核心理论图景
  4. RSEA (arXiv:2606.28374) — 安全门控的必要性证明

参考:
  5. SkillGrad (arXiv:2605.27760) — 梯度类比
  6. UCE (arXiv:2606.02304) — 上下文类型学
  7. EDV (arXiv:2606.24428) — 多 agent 验证
  8. SkillEvolBench (arXiv:2605.24117) — 提炼质量基准
  9. ICL as Gradient Descent (Dai 2022) — 理论基础
  10. awesome-agent-evolution — 持续追踪列表
```

---

## 附录：搜索方法

本次调研通过以下渠道进行（2026-06-30）：
- arXiv 论文搜索
- GitHub 仓库搜索（按 stars 排序）
- 微信搜狗搜索（中文内容）
- 百度/CSDN（中文内容）
- Semantic Scholar API（部分受限）
- LessWrong / Substack（部分受限）

因网络限制未成功访问：Twitter/X、Reddit、知乎、Medium。
