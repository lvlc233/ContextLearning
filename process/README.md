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

- [01 数据集准备](01-dataset-preparation.md)
- [02 训练目标](02-training-targets.md)
- [03 训练方向与数据标注](03-training-direction-and-labeling.md)
- [04 训练计划与参数](04-training-plan-and-parameters.md)
- [05 训练](05-training.md)
- [06 测试与评估](06-testing-and-evaluation.md)
