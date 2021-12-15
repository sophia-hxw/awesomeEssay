

# 对比faster rcnn
Mask rcnn网络是基于faster rcnn网络架构提出的新的目标检测网络。该网络可以在有效地完成目标检测的同时完成实例分割。Mask RCNN主要的贡献在于如下：
- 1.强化了基础网络：通过ResNeXt-101+FPN用作特征提取网络，达到state-of-the-art的效果。
- 2.ROIAlign替换之前faster rcnn中的ROI Pooling，解决错位（Misalignment）问题。
- 3.使用新的Loss Function：Mask RCNN的损失函数是分类，回归再加上mask预测的损失之和。

总结来说，mask rcnn的主要贡献就是采用了ROI Align以及加了一个mask分支。
