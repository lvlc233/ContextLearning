# Context Learning

这是一个围绕“上下文学习 / 静态上下文训练”的文档项目。

当前目标不是填充大量内容，而是把已有材料整理成可持续迭代的结构。

## 文档入口

- [主文档](document.md)
- [概念层](concepts/README.md)
- [上下文训练流程](process/README.md)
- [测试流程](notes/process/testing-flow.md)
- [algo-coach 训练案例](cases/algo-coach/README.md)
- [计划表](notes/plan.md)

## 当前结构

```text
Context Learning/
  README.md
  document.md
  concepts/
    README.md
    001-overview.md
    002-context-training.md
    003-boundaries.md
    004-training-signals.md
    005-visualization.md
  process/
    README.md
    001-basic-flow.md
    002-dataset-and-preparation.md
    003-training-process-and-parameters.md
    004-training-targets.md
    005-data-labeling.md
    006-training.md
    007-post-training.md
    008-checkpoint.md
  cases/
    README.md
    algo-coach/
      README.md
      training-plan.md
      training-targets.md
      labels.md
      materials/
        README.md
      outputs/
      checkpoint/
  notes/
    plan.md
    checkpoint.md
    process/
      README.md
      testing-flow.md
```

## 当前整理原则

- 主文档保留方法论主线。
- 流程层从主文档拆出，单独放入 process。
- 案例独立出来，避免主方法论被具体案例拉偏。
- 案例材料放在案例内部，避免材料和方法论混在一起。
- 尚未稳定的方法流程先放入 notes，等待后续沉淀。
- checkpoint 相关方法先放入 notes，案例自己的 checkpoint 放入案例目录内部。
