# checkpoints

这里存放上下文训练过程中的 checkpoint。

checkpoint 用于记录某个阶段的上下文版本、训练材料、输出结果和偏离情况。

推荐记录：

- 版本号
- 对应上下文本体
- 使用的数据集
- 生成输出
- 人类标注
- 主要偏离
- 下一轮调整方向

## 建议结构

```text
checkpoints/
  README.md
  YYYY-MM-DD-v0.md
  YYYY-MM-DD-v1.md
```

checkpoint 不一定记录每一次小修改。

更适合记录：

- 一个结构阶段完成后；
- 一轮训练验证完成后；
- 一个上下文版本可以被回退或复用时。
