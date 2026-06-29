# 基本流程

可以把流程拆成六个主要步骤。

```text
→ 数据集准备
→ 制订/提炼 训练目标
→ 制订训练方向与数据标注(参考奖励信号/奖励函数)
→ 设计训练计划与参数
→ 训练
→ 测试与评估
```

每个过程都可以独立循环进行,也可以构建为一个更大的循环结构运行。

其中 `后训练` 和 `checkpoint` 当前归入 `训练` 章节。它们直接服务于训练过程中的偏好校准、版本保存与回退,暂不作为独立流程文件。

## 目录

- [001 数据集准备](001-dataset-preparation.md)
- [002 训练目标](002-training-targets.md)
- [003 训练方向与数据标注](003-training-direction-and-labeling.md)
- [004 训练计划与参数](004-training-plan-and-parameters.md)
- [005 训练](005-training.md)
- [006 测试与评估](006-testing-and-evaluation.md)
