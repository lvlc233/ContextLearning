# 理论迁移清单

这份清单用于暂存已经在会话或参考文档中出现、但尚未充分迁移到 `concepts/` 的理论内容。

它不是最终理论正文。它的作用是：

- 防止会话中已经形成的概念丢失；
- 标记每个概念的证据来源；
- 判断它应该进入 `concepts/`、`process/`，还是案例材料；
- 给后续逐篇展开提供顺序。

## 迁移原则

- 先记录概念，不急着写成规则。
- 优先迁移能解释多个现象的概念。
- 不把案例原样塞进理论，只提炼它支持的判断。
- `process/` 保留“怎么做”，`concepts/` 解释“为什么这样做”。
- 对证据不足的概念标记为待确认，不写成已定论。

## 高优先级

| 概念 | 当前理解 | 主要证据 | 建议落点 | 状态 |
|---|---|---|---|---|
| 概念到操作 | 概念不能直接塞进 skill，需要转成 agent 可执行、可测试、可失败、可修正的动作规则。 | 会话中从“概念很好听但落不到行为”推进到“小型操作化”；`feedback-mode-evolution.md` 第四层。 | `concepts/core/003-concept-to-operation.md` | 已迁移初版 |
| 先反馈，再提问 | 有明确外部标准的训练场景里，agent 应先判断用户输入，再决定是否追问。 | `algo-coach` 机械追问问题；105/102 会话中用户要求“先说对错”。 | `concepts/experience/005-feedback-first.md` | 已迁移初版 |
| 反馈质量 | 好反馈要贴住用户具体输入，指出哪里对、哪里错、错误重要程度，而不是泛泛说“方向对”。 | 102、199、230 的反馈质量拟输出与人类标注。 | `concepts/experience/006-feedback-quality.md` | 已迁移初版 |
| 尺子式反馈 | 反馈应像老师拿尺子指出具体偏差：先指用户代码/语句，再解释它实际意味着什么。 | 230 中“访问过程中的计数/子树递归返回数量”被指出太抽象。 | `concepts/experience/006-feedback-quality.md` | 已迁移初版 |
| 连续性与聚焦性 | 好引导要沿着用户当前注意力推进，不应在用户还在处理上文时追加多个新问题。 | 会话中用户提出“连续性和聚焦性”；102 学习过程中的“不要问没有意义的问题”。 | `concepts/experience/007-continuity-and-focus.md` | 已迁移初版 |
| 扩散后收捻 | 扩散不是问题，问题是打开分支后不整理、不关闭、不回到主线。 | 会话中“agent 内容扩张 / 人类回复有限”与“扩散后不梳理”。 | `concepts/experience/008-divergence-and-convergence.md` | 已迁移初版 |
| 案例风险 | 案例是危险订正材料，直接注入会导致表层模仿、过拟合、浅学习和上下文污染。 | 训练后期用户明确指出“案例是最危险的订正”；现有 `004-training-signals.md` 已有雏形。 | `concepts/experience/009-case-risk.md` 或扩展 `004-training-signals.md` | 已迁移初版 |

## 中优先级

| 概念 | 当前理解 | 主要证据 | 建议落点 | 状态 |
|---|---|---|---|---|
| 偏好信号 | 人类反馈不只是好/坏评价，而是可转成训练方向和行为规则的信号。 | `process/005-data-labeling.md`；102/101/98 的偏好标注。 | `concepts/core/004-preference-signal.md` | 已迁移初版 |
| 偏好形成集 | 已被人类标注并用于形成规则的样本，不能再强证明泛化能力。 | 会话中将 102/230/199 定义为偏好形成集。 | `process/009-testing-and-evaluation.md`，理论摘要进 `concepts` | 已迁移到流程初版 |
| 未见测试集 | 未参与规则形成的样本，用于检查规则是否泛化。 | 101/148/98 未见样本测试。 | `process/009-testing-and-evaluation.md`，理论摘要进 `concepts` | 已迁移到流程初版 |
| 单维测试 | 每轮只测试一个维度，避免结果不可解释。 | 用户指出“每次只测试一个维度比较好”；反馈质量/引导方向分开测试。 | `process/009-testing-and-evaluation.md` | 已迁移到流程初版 |
| 正交测试与条件测试 | 独立测试不能建立在另一个能力已达标的前提上；条件测试需要显式说明。 | 会话中用户纠正“在反馈达标基础上测引导”不是正交测试。 | `process/009-testing-and-evaluation.md` | 已迁移到流程初版 |
| 训练目标正交性 | 目标应尽量互不干扰，方便判断改动来源和并行训练。 | `process/004-training-targets.md` 已提出，但没有展开。 | `concepts/core/005-training-target.md` | 已迁移初版 |
| 等待动作 | 等待不是缺少内容，而是给用户继续组织思考的空间。 | 会话中用户指出人类交流会一条条说，需要等待。 | `concepts/experience/007-continuity-and-focus.md` | 已迁移初版 |

## 流程层理论混入

这些内容目前写在 `process/`，但实际承担了理论解释职责。后续应保留流程摘要，并把完整解释迁入 `concepts/`。

| 文件 | 混入的理论内容 | 建议 |
|---|---|---|
| `process/002-dataset-and-preparation.md` | 数据单位、训练/测试/标注/偏好/目标集的边界。 | 流程保留，概念层补“数据集类型”。 |
| `process/003-training-process-and-parameters.md` | 将训练流程与参数类比为上下文训练的超参数。 | 流程保留，概念层补“训练流程与参数”。 |
| `process/004-training-targets.md` | 训练目标来自偏好，目标要落到行为偏差，并考虑正交性。 | 概念层优先迁移。 |
| `process/005-data-labeling.md` | 自然语言标注、优化漂移、“为了修改而修改”、偏好信号转规则。 | 概念层优先迁移。 |
| `process/007-post-training.md` | 后训练类似偏好微调/专家校准，而不只是一个步骤。 | 概念层补定义。 |

## 暂缓迁移

| 内容 | 暂缓原因 |
|---|---|
| checkpoint 理论化 | 当前更像流程管理与版本保存，先保留在 `process/008-checkpoint.md`。 |
| 轻量模型缩放边界 | 已是测试策略，等 `process/009-testing-and-evaluation.md` 成型后再判断是否独立成理论。 |
| 资源包 / 偏好蒸馏 | 会话中只是候选想法，证据不足，先标记为待观察。 |
| 动态上下文学习 | README 只提到方向，当前项目主要处理静态上下文训练。 |

## 下一步建议

1. 先补 `concepts/core/003-concept-to-operation.md`。
2. 再补 `concepts/experience/006-feedback-quality.md`。
3. 然后补 `concepts/experience/007-continuity-and-focus.md`。
4. 最后整理 `concepts/experience/009-case-risk.md`，并决定是否拆分现有 `004-training-signals.md`。
