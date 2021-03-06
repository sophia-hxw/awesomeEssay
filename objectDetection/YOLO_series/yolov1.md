## 问题背景

之前 two-stage 方法如 R-CNN 把检测问题分成两部分，先生成候选区域（region proposal），再用分类器对区域分类，多阶段训练导致不易优化。

## 创新点

把检测当作回归问题，用一个网络输出位置和类别，实现了一个 unified system，从检测的角度是 one-stage的

## 训练流程

- 和 R-CNN 差不多   

- 首先 ImageNet 1000类 竞赛数据集上对卷积层进行预训练，然后再把网络根据检测任务微调

## 检测流程

- 输入一幅多目标图像   
- 将图像划分成多个网格   
- 通过网络得到每个网格的分类概率，以及各网格预测的框+置信度   
- 针对每个框，把概率与置信分数相乘，作为每个框特定于每个类的置信分数   
- 输出位置和类别信息   

## 优点

- 快。因为回归问题没有复杂的流程（pipeline）。    
- 可以基于整幅图像预测（看全貌而不是只看部分）。   
  与基于滑动窗口和区域提议的技术不同，YOLO在训练和测试期间会看到整个图像，因此它隐式地编码有关类及其外观的上下文信息。因为能看到图像全貌，与 Fast R-CNN 相比，YOLO 预测背景出错的次数少了一半。
- 学习到物体的通用表示（generalizable representations），泛化能力好。    
  因此，当训练集和测试集类型不同时，YOLO 的表现比 DPM 和 R-CNN 好得多，应用于新领域也很少出现崩溃的情况。

## 缺点

- 空间限制    
  一个单元格只能预测两个框和一个类别，这种空间约束必然会限制预测的数量；
- 难扩展    
  模型根据数据预测边界框，很难将其推广到具有新的或不同寻常的宽高比或配置的对象。由于输出层为全连接层，因此在检测时，YOLO 训练模型只支持与训练图像相同的输入分辨率。
- 网络损失不具体    
  无论边界框的大小都用损失函数近似为检测性能，物体 IOU 误差和小物体 IOU 误差对网络训练中 loss 贡献值接近，但对于大边界框来说，小损失影响不大，对于小边界框，小错误对 IOU 影响较大，从而降低了物体检测的定位准确性。