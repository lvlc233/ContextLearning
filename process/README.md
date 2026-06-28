# 基本流程

可以把流程拆成六个步骤。

```text
→ 数据集准备
→ 设置训练流程与参数
→ 制订/提炼 训练目标 (参考目标函数/奖励信号)
→ 受人类标注[偏好]的前训练/数据标注(可选但推荐)
→ 训练
→ 受人类标注[偏好]的后训练(可选)
```

每个过程都可以独立的循环进行,也可以构建为一个更大的循环结构进行运行;

## 目录

- [002 数据集与准备](002-dataset-and-preparation.md)
- [003 训练流程与参数](003-training-process-and-parameters.md)
- [004 训练目标](004-training-targets.md)
- [005 数据标注](005-data-labeling.md)
- [006 训练](006-training.md)
- [007 后训练](007-post-training.md)
- [008 checkpoint](008-checkpoint.md)
- [009 测试与评估](009-testing-and-evaluation.md)

